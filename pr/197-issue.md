title:	[Bug] 公開閲覧のUser-Agent・接続元IPやDIのエラーメッセージが列の長さを超えると500になる
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
number:	197
--
### 事象

APIは、一部の値をDBの列の長さを確かめずに保存する。値が列の長さを超えると、PostgreSQLが`DataError`を返し、APIは500 `internal-error`を返す。

公開閲覧のページ寸法・ページ画像・PDF（`/view/{token}/pages`・`/view/{token}/pages/{page}/image`・`/view/{token}/pdf`）は、リクエストの`User-Agent`を閲覧イベントの`user_agent`（`varchar(512)`）にそのまま保存する。513文字以上の`User-Agent`で閲覧すると応答が500になり、文書を閲覧できない。閲覧イベントも記録されない。

公開閲覧は、接続元IP（`request.client.host`）もそのまま保存する。保存先は閲覧イベントの`client_ip`と、無効なトークンの失敗記録の`client_ip`で、どちらも`varchar(64)`である。uvicornは既定で、接続元が127.0.0.1の要求に限り、`X-Forwarded-For`の値を接続元IPにする。そのため、127.0.0.1から65文字以上の`X-Forwarded-For`を付けて閲覧すると、応答が500になる。トークンが無効な場合も、404ではなく500になる。ローカルでは、Viewerの開発サーバ（Viteのプロキシ）を経由しても起きる。本番のContainer Appsでは起きない見込みである。利用者が送った`X-Forwarded-For`の値は接続元IPにならないと考えているが、Azure上では確かめていない。`docs/architecture/view-url.md`の「`X-Forwarded-For`は見ない」という記述も、uvicornのこの挙動と合っていない。

IDP実行（`POST /api/v1/documents/{document_id}/idp-runs`）は、DIの呼出に失敗したとき、例外のメッセージを`error_info`（`varchar(2048)`）に保存する。メッセージが2049文字以上だと、502 `document-analysis-failed`ではなく500になり、失敗した実行も記録されない。Azure SDKの例外のメッセージがこの長さに達するかは、確かめていない。

#166 の調査（APIとViewerの入力検証の有無）で見つかった。#166 とは原因が違うため、別のIssueにした。

### 再現手順

1. 共有できる文書種別（見積書など）の文書を登録し、閲覧URLを発行する
2. 600文字の`User-Agent`を付けて、`GET /view/{token}/pages`を送る
3. APIを動かしているホストから`127.0.0.1`宛てに、100文字の`X-Forwarded-For`を付けて`GET /view/{token}/pages`を送る。存在しないトークンで送っても、同じ結果になる
4. APIのテストで使う`FakeDocumentAnalyzer`に、3000文字のメッセージを持つ`DiError`を設定する。その状態で`POST /api/v1/documents/{document_id}/idp-runs`を送る

### 期待する動作

閲覧は、`User-Agent`と接続元IPの値によらず成功し、閲覧イベントを記録する。`User-Agent`は、列の長さ（512文字）までに切り詰めて保存する。接続元IPは、IPアドレスとして解釈できない値を`unknown`として保存する。トークンが無効なら404を返し、失敗を記録する。

DIの呼出に失敗したときは502を返し、失敗した実行を記録する。例外のメッセージは、切り詰めずに全文を保存する。

### 実際の動作

手順2では、500 `internal-error`が返る。サーバのログには、次のエラーが出る。

```
psycopg.errors.StringDataRightTruncation: value too long for type character varying(512)
```

手順3でも500 `internal-error`が返り、`character varying(64)`について同じエラーが出る。閲覧イベントも失敗記録も残らない。

手順4でも500 `internal-error`が返り、`character varying(2048)`について同じエラーが出る。`idp_runs`には行が残らない。

### 対象コンポーネント（複数選択可）

API（`api` 配下）, ドキュメント（`docs` 配下）

### 環境

ローカル（PostgreSQL）で再現した。手順3は、Viewerの開発サーバ（Vite）経由でも再現した。手順4は、APIのテストと同じ構成（テスト用のフェイク）で確かめた。

