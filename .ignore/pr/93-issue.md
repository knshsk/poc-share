title:	[Feature] 閲覧イベントに閲覧ページ番号（page_view）を追加
state:	CLOSED

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
number:	93
--
### 目的

取引先（社外）に見積PDFを開示した際、取引先が何ページ目まで確認したかを把握したい。現在の閲覧ログは閲覧用URLへのアクセス（view）と原本DL（download）のみで、ページ単位の閲覧事実を記録していない。

### 変更内容

- `view_events` に `page` 列（integer NULL）と種別 `page_view` を追加する
- `GET /view/{token}/pages/{page}/image` の配信成功時に `page_view`（ページ番号付き）を記録する
- 外部連携フィード（`/api/view-events`、`/api/documents/{id}/events`）の応答に `page` 項目を追加し、`page_view` を含めて返す
- Viewer の `PageSvg` を表示領域に入った時のみ画像取得する遅延読込に変更する（画像取得＝ページ表示）

### 対象コンポーネント（複数選択可）

- API（`api` 配下）
- 画面（`viewer` 配下）
- ドキュメント（`docs` 配下）

### 完了条件

- [ ] 閲覧用URL経由のページ画像取得で `page_view` が `page` 付きで記録される
- [ ] `/api/view-events` と `/api/documents/{id}/events` の応答に `page` が含まれる（`view` / `download` は null）
- [ ] PublicView で表示領域に入っていないページの画像が取得されない
- [ ] `docs/data-model/view-events.md`、`docs/requirements/viewer.md`、`docs/api/openapi.json` を更新した
- [ ] ADR（`docs/adr/0015-page-view-event.md`）を作成した
- [ ] pre-commit・`uv run pytest`・`npm run test` が通過している

### 関連情報

- ADR-0005（追記型ログ）、ADR-0012（社内閲覧の非履歴化）、ADR-0013（差分フィード）
