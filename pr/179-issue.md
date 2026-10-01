title:	[Bug] 文書 ID に日本語を含む文書の原本ダウンロードが 500 エラーになる
state:	CLOSED
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
number:	179
--
### 事象

文書 ID に Latin-1 の範囲外の文字（日本語など）を含む文書は、原本 PDF のダウンロードが 500 エラーになる。対象は `GET /api/v1/documents/{document_id}/pdf`（Viewer の [原本ダウンロード]）と `GET /view/{token}/pdf`（閲覧 URL の [ダウンロード]）。文書の登録とページの表示はできる。#178 の調査中に見つけた。

原因は、`pdf_response`（`api/app/routers/documents.py`）が `Content-Disposition` の `filename` パラメータに文書 ID をそのまま埋め込んでいること（`attachment; filename="{document_id}.pdf"; filename*=...`）。Starlette は応答ヘッダを latin-1 で符号化するため、範囲外の文字で例外になる。文書 ID の文字種は、API でも Viewer の登録フォームでも制限していない。

文書 ID に `"` を含む場合は、500 にはならないが、`filename="q"id.pdf"` のように引用の崩れたヘッダを返す。

### 再現手順

1. `POST /api/v1/documents` で `document_id` を `見積-001` にして文書を登録する（201 で成功する）
2. `GET /api/v1/documents/見積-001/pdf` を呼ぶ

### 期待する動作

- 文書 ID の文字種によらず原本をダウンロードできる。保存名は従来どおり原本ファイル名（無ければ `{document_id}.pdf`）

### 実際の動作

500 エラーになる。API のテストクライアントで再現したときの例外:

```
UnicodeEncodeError: 'latin-1' codec can't encode characters in position 22-23: ordinal not in range(256)
```

対応方針: `filename` には、文書 ID の ASCII の印字可能文字以外と `"`・`\` を `_` に置き換えた代替名を入れる。実際の保存名は常に `filename*`（RFC 5987 の UTF-8 表記）で渡す。

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 環境

ローカル（API のテストクライアントとテスト用 DB）。`main` で確認。

