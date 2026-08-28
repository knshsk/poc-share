title:	[Feature] API: 閲覧ログの差分取得インタフェースの提供
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
number:	63
--
### 目的

業務フロー K3-9（`docs/business/operation-flow.md`）では、Power Automateが日次バッチで閲覧ログを「差分取得」し、文書IDから案件IDを解決してDataDeliveryへ登録する（要件 VW-04）。現実装の `GET /api/documents/{document_id}/events` は文書ID指定・全件返却のみで、差分取得の手段がない。また連携側が対象文書IDの一覧を知る手段（文書一覧APIやイベント横断取得）も提供していない。業務フローを正とし、差分取得可能なインタフェースを提供する。

### 変更内容

- 閲覧イベントの横断取得エンドポイントの追加（発生日時ベースの差分指定、例: `since` パラメータ）
- 冪等キー（文書ID＋イベント発生日時）で連携側が重複排除できる応答項目の維持
- 設計方式の決定（横断+差分指定エンドポイント追加か、既存の文書ID単位契約＋連携側での文書ID管理の明文化か）— 決定はADR起票
- `docs/data-model/view-events.md` の外部連携IF記載を決定内容へ更新

### 対象コンポーネント（複数選択可）

- API（`api` 配下）
- ドキュメント（`docs` 配下）

### 完了条件

- [ ] Power Automateが日次バッチで前回取得以降の閲覧イベントを取得できるIFが提供されている
- [ ] 取得方式の決定がADRとして記録されている
- [ ] `docs/data-model/view-events.md` が更新されている
- [ ] イベント記録・取得（差分境界・異常系含む）のテストを添付している

### 関連情報

- 要件: `docs/requirements/viewer.md` VW-04
- 業務フロー: `docs/business/operation-flow.md` K3（K3-9）
- 設計: `docs/data-model/view-events.md`（外部連携IF-04）

## Comments

