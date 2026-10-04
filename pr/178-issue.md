title:	[Feature] 文書登録APIに原本ファイル名のフィールドを追加する
state:	CLOSED
author:	shiro-ino (しろいの)
labels:	feature
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
number:	178
--
### 目的

Power Automateから文書登録 API（`POST /api/v1/documents`）へ PDF を送ると、日本語のファイル名が `?` に化けて保存される（`ABCあいう.pdf` → `ABC???.pdf`）。原本ファイル名は Viewer のヘッダーの文書タイトルと原本ダウンロードの保存名に使うため、利用者が文書を見分けられない。

PA の HTTP アクションは、multipart のパートヘッダ（`Content-Disposition` の `filename`）に含まれる非 ASCII 文字を、送信前に 1 文字ずつ `?` に置き換えるものと思われる。公式ドキュメントに記載はないが、Logic Apps でも同じ症状が報告されている。

リクエスト全体に `charset=utf-8` を指定しても直らず、PA 側の設定では回避できない。一方、パートの本文（テキスト）は UTF-8 のまま届く（日本語の `document_id` を PA から登録し、応答の `document_id` が化けないことを実機で確認した）。

API 側でパートヘッダの `filename` を percent-decode する案は採らない。`%20` のような文字列を含む実際のファイル名が復号で失われ、全クライアントの `filename` の解釈が変わるため。

### 変更内容

- `POST /api/v1/documents` に任意のテキストフィールド `original_filename` を追加する。値があれば、`file` パートの `filename` より優先して原本ファイル名に使う
- 正規化は `filename` と同じ（パス区切り `/` `\` より後ろだけを取り、前後の空白を除き、255 文字で切り詰める）。空・空白のみのときは `filename` を使う
- 冪等な再送（同一内容で 200）では、既存の原本ファイル名を上書きしない（現行どおり）
- Viewer の画面登録は変えない（ブラウザは `filename` を UTF-8 のまま送れる）
- OpenAPI（`docs/api/openapi.json`）と TS 型を再生成する
- ドキュメント: `docs/data-model/documents.md`（`original_filename` 列の説明）、`docs/architecture/api.md`（文書登録の流れ）。ADR を起票する

PA がリクエストヘッダに非 ASCII を載せられない前提で、PA が API を呼ぶときのヘッダを調べた。自由な文字列が入るのは multipart の `filename` だけだった。`Authorization` はアクセストークン、パート名（`name`）は固定の ASCII で、`document_id`・`document_type` はパートの本文で送る。API はカスタムヘッダを受け付けていない。

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- `original_filename` を指定した登録で、その値が原本ファイル名として保存され、応答の `original_filename` と原本ダウンロードの保存名に使われる
- `original_filename` を指定しない登録は、従来どおり `filename` から原本ファイル名を取る
- 空・空白のみ・パス付きの `original_filename` の扱いをテストで確認する
- OpenAPI と TS 型を再生成し、ドキュメントと ADR を更新する

### 関連情報

- 原本ファイル名の保存: `documents.original_filename`（`docs/data-model/documents.md`）

