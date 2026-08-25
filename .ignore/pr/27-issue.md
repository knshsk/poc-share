title:	[Feature] 閲覧・DLイベント記録
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
number:	27
--
### 目的

閲覧URL経由の閲覧・ダウンロードを文書ID・イベント種別・発生日時・端末情報とともに欠損・重複なく記録する。

### 変更内容

- `view_events` テーブル（追記型）: id / document_id(FK) / view_token_id(FK) / event_type(view・download) / occurred_at / client_ip / user_agent
- UNIQUE(document_id, occurred_at, event_type) で冪等キー（文書ID+イベント発生日時）をDB制約として担保。衝突時は挿入スキップ
- 記録ポイント（公開閲覧経路のみ・配信成功時）:
  - `GET /view/{token}/pages` 成功 → view イベント（ページ画像の個別取得は記録しない）
  - `GET /view/{token}/pdf` 成功 → download イベント
- 社内（認証済み）表示は記録しない（社内閲覧イベントの履歴化要否が未確定のための仮置き）
- `GET /api/documents/{document_id}/events`（JWT必須）— 記録一覧（検証用）
- 外部連携（日次取得）は取得方式未確定のためスコープ外

### 完了条件

- [ ] 閲覧・DLが種別・端末情報・トークン紐付きで記録される
- [ ] 配信成功時に必ず1件記録され、同一冪等キーの重複記録がない（テストで担保）
- [ ] 無効トークンのアクセスは記録されない
- [ ] pre-commit・pytest・Vitest・API契約差分チェックすべて通過

### 関連情報

- スコープ外: イベント外部連携、社内閲覧の記録

## Comments

