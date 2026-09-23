title:	[Bug] 文書登録でファイル名に制御文字を含むと 500 になり blob だけが残る
state:	OPEN

labels:	bug
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	166
--
### 事象

文書登録（`POST /api/v1/documents`）で、multipart の `filename` に NUL などの制御文字を含むリクエストを送ると 500 になり、DB には行が作られないまま blob だけが保存される（孤児 blob）。

- `_original_filename`（`api/app/routers/documents.py`）は basename の取り出しと 255 文字への切り詰めだけを行い、制御文字・bidi 制御文字を除去しない
- 登録処理は `blob_storage.save(...)` を `session.commit()` より前に呼ぶため、commit が失敗すると blob が残る
- Azure の `AzureBlobStorage.save` は `upload_blob(..., overwrite=False)` のため、孤児 blob が残った `document_id` は、以後の登録が毎回 500 になる見込み（コードからの推定。Azure では未実行）

PR #165 の作業中に行った再レビュー（#160 / PR #163 の再確認）で見つかった。#160 の差分より前からある挙動。

### 再現手順

1. 認証済み（同一テナント）のクライアントから、生の multipart で `filename="a\x00b.pdf"` を指定して `POST /api/v1/documents` を送る（httpx / TestClient の `files=` は制御文字を percent-encode するため再現しない。生のリクエストボディを組み立てる）
2. 応答を確認する
3. 同じ `document_id` で `GET /api/v1/documents/{document_id}` を送る

### 期待する動作

- 制御文字（`\x00`-`\x1f`、`\x7f`）と bidi 制御文字（`U+202A`-`U+202E`、`U+2066`-`U+2069`）を除去したファイル名で登録できる（除去後に空なら `original_filename = null`）
- DB への保存に失敗した場合に blob が残らない（または、残っても同じ `document_id` の再登録が成功する）

### 実際の動作

- PostgreSQL が `DataError`（文字列に NUL を含められない）を返し、API は 500 `internal-error` を返す
- blob は保存済みのまま残り、続く GET は 404（ローカルの再現で確認）
- 補足: TAB はそのまま通る。`filename*=` は python-multipart が解釈せず 422。RLO などの bidi 制御文字はそのまま保存され、Viewer のヘッダーと `Content-Disposition` の保存名で表示順を偽装できる

影響範囲: 認証済みの同一テナントのユーザーが細工したリクエストを送った場合に限る。深刻度は低いが、blob 残存と「その `document_id` が使えなくなる」という永続的な副作用がある。

修正案（再レビューの提案）:

```python
_UNSAFE = re.compile(r"[\x00-\x1f\x7f‪-‮⁦-⁩]")
name = _UNSAFE.sub("", re.split(r"[\\/]", filename or "")[-1]).strip()
```

あわせて、blob の保存と commit の順序（commit 失敗時の blob の後始末、または保存を commit の後にする）を検討する。テストは `_original_filename("a\x00b.pdf")` の単体テストが確実。

類似不具合は未調査。Viewer 、 API ともにバリデーション有無の調査を実施してから最終的な修正案を検討する。

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 環境

ローカル（PostgreSQL + LocalBlobStorage）で再現。Azure での挙動（同じ `document_id` が以後 500）はコードからの推定。

