title:	[Feature] Viewer: ヘッダーに原本ファイル名を文書タイトルとして表示する
state:	CLOSED

labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#159
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	160
--
### 目的

アプリバーの文書 ID バッジと種別チップは、ロゴの横にタグのように置かれていて浮いて見える。また利用者にとっては文書 ID より原本ファイル名のほうが、開いている文書を判別しやすい。原本ファイル名は DB（`documents.original_filename`）に保存済みだが、文書メタデータの応答には含まれていない。

### 変更内容

- API: `DocumentResponse`（`POST /api/v1/documents`・`GET /api/v1/documents/{document_id}` で共用）に `original_filename: str | None` を追加する。DB 列は既存のためマイグレーションは不要。項目の追加のみで後方互換。`npm run generate:api` で `docs/api/openapi.json` と `viewer/src/api/schema.d.ts` を再生成する
- Viewer: `LoadedDocument.originalFilename` と `HeaderVm.documentName` を追加し、文書取得の応答から入れる
- `AppHeader`: ロゴ | 縦の区切り線 | テキストボタン [種別チップ + 主表示 + 文書 ID（小・等幅）+ ⌄]
  - 主表示は原本ファイル名。無い文書（ファイル名なしの登録経路）と読込中は文書 ID を主表示にする。ファイル名があるときだけ文書 ID を横に添える
  - 長いファイル名は省略記号で切り、`title` 属性に全文を入れる
  - クリックで現行の文書 ID 再入力メニューを開く（挙動・`data-testid` は現行どおり）
  - 公開閲覧（`/view/{token}`）はロゴのみのまま
- ドキュメント: `docs/viewer/display.md`（ヘッダー）、`docs/data-model/documents.md`、`docs/architecture/viewer.md`

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 文書メタデータの応答に `original_filename` が入る（ファイル名なしの登録は null）。API テスト添付
- ヘッダーに種別 + 原本ファイル名 + 文書 ID がタイトルとして表示され、ファイル名が無い文書は文書 ID が主表示になる。テスト添付
- 文書 ID の再入力メニューと公開閲覧のロゴのみ表示が維持されている
- `uv run pytest` / `npm run test` / `npm run lint` / `npm run build` と pre-commit（API 契約の差分チェックを含む）通過

### 関連情報

- 親: #159
