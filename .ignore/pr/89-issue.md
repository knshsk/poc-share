title:	[Chore] 業務フローの更新
state:	CLOSED

labels:	chore
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
number:	89
--
### 目的

全体設計の更新により業務フローに一部変更が生じたため、リポジトリ内のドキュメントについても更新が必要となった。

### 作業内容

- `docs/business/operation-flow.md` に記載の業務フローを最新版に更新
- 業務フロー更新に伴う波及箇所の修正
  - `docs/business/operation-flow.md` レーン定義表（レーン4: WBS原価データ出力の削除、レーン10: 予測保管・凍結の記述をF2〜F4に合わせて更新）、K2-9のWBS記述削除
  - `docs/requirements/idp.md` IDP-01: WBS原価データを対象から削除（K2-3で見積PDFのみ出力に変更）
  - `docs/business/out-of-scope.md` OUT-09: 型区分判定キーを「品目マスタ」から「注文書に紐づく見積データの有無」に変更（K4-7に合わせる）
  - 用語統一: 「Engagement History」→「案件型顧客エンゲージメント履歴」、「OrderHistory」→「発注実績」、「判定キー」→「名寄せキー」（`docs/business/factone-concept.md`・`docs/requirements/viewer.md`）
  - `docs/requirements/viewer.md` VW-04: 出所工程の冪等キー記述を削除（業務フローK3-9から削除。冪等キー自体はADR-0005/0013・`docs/data-model/view-events.md` の設計判断として維持）

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- 全体設計とリポジトリ内の業務フローに差異がないこと
- 業務フローと `docs/requirements/`・`docs/business/` 配下の他ドキュメントに矛盾がないこと

### 関連情報

以下は本Issueでは対応せず、判断・外部回答待ち。確定後に別Issueで対応する。

- **OUT-16（翌月第2営業日・月中仮想／月末物理の凍結）**: 旧F3-1の凍結記述が新版F3-7「計上月別スナップショットへ保管 ※担当者検討中（9/1現在回答待ち）」に置換され、OUT-16の前提が業務フローから消えた。担当者回答後にOUT-16の合意状況・縮退仕様を見直す。
- **WBS対象外のOUT項目新設**: 新版K2-3でWBS原価データ出力が削除、K5-10注記「WBS情報のSAP連携は今回対象外」。`docs/business/out-of-scope.md` に対応項目がない。全体設計側のout-of-scope一覧にOUT番号があれば転記、なければ新設（例: WBS原価データの読取・SAP連携は行わない）。レーン5「SAPがWBS採番」・`docs/business/factone-concept.md` のWBS言及（L17/59/63）も併せて見直す。
- **K3-2 WalkMe⇒Viewer連携方式（確認事項）**: 案1 REST APIで閲覧URL発行／案2 Viewer画面起動＋ボタン発行→WalkMe読取。現要件VW-01/VW-07は案2前提。案1採用ならWalkMeから呼べる閲覧URL発行APIが要件追加となり、既存API（`POST /api/documents/{id}/view-tokens` 相当）の外部公開可否が論点。確定まで要件は据え置き。

