title:	[Feature] 書類種別設定のkey/type_codeバリデーションで大文字を許可する
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
number:	103
--
### 目的

Document Intelligence の公式ドキュメントでは Query Fields のキーがパスカルケースで記載されている。現在の書類種別設定は `key` を `^[a-z0-9_]{1,50}$` に制限しており、ドキュメントに倣ったキー名を登録できない。

### 変更内容

- `api/app/routers/document_type_configs.py` の `_CODE_PATTERN` を `^[a-z0-9_]{1,50}$` から `^[A-Za-z0-9_]{1,50}$` に変更する
- 定数は `key` と `type_code` で共有しているため、`type_code` も大文字を許可する
- `docs/data-model/document-type-configs.md` の正規表現表記を更新する
- `docs/api/openapi.json` を再生成する

### 対象コンポーネント（複数選択可）

API（`api` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] PascalCase の `key`（例: `InvoiceNo`）で文書種別設定を登録できる
- [ ] ハイフン等の不正文字は引き続き 422 になる
- [ ] テスト追加済み
- [ ] ドキュメント・openapi.json 更新済み

### 関連情報

- ADR-0014
