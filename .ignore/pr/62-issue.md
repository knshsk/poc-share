title:	[Feature] Viewer: 閲覧用URL発行・管理UIの追加
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
number:	62
--
### 目的

業務フロー K3-2〜K3-3（`docs/business/operation-flow.md`）では、営業がCRM画面からWalkMe経由でViewerを起動し、画面上で見積PDFの閲覧用URLを受け取る（要件 VW-01）。発行・一覧・失効のAPIは実装済みだが、Viewer画面に発行UIがなく、この業務フローを画面から実行できない。

### 変更内容

- `DocumentView` に閲覧用URLの発行ボタンと発行結果（URL）の表示を追加
- 発行済みURLの一覧表示と失効操作のUIを追加
- 閲覧用URLを発行できない文書種別（注文書）ではUIを非表示または無効化（顧客開示禁止の境界を画面でも担保）

### 対象コンポーネント（複数選択可）

- 画面（`viewer` 配下）

### 完了条件

- [ ] 見積文書に対し画面から閲覧用URLを発行し、URLをコピーできる
- [ ] 発行済みURLの一覧確認と失効が画面からできる
- [ ] 注文書種別では発行UIが提供されない
- [ ] コンポーネントテストを添付している

### 関連情報

- 要件: `docs/requirements/viewer.md` VW-01
- 業務フロー: `docs/business/operation-flow.md` K3（K3-2〜K3-3）
- ADR: `docs/adr/0011-view-url-opaque-token.md`
- 既存API: `POST /api/documents/{document_id}/view-urls`、`GET 同`、`POST /api/view-urls/{id}/revoke`

## Comments

