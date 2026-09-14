title:	[Chore] 外部システム担当者との調整待ち事項を確定後に docs/business へ反映する
state:	OPEN
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
number:	111
--
### 目的

全体設計レビューで見つかった業務フロー・PoC対象外事項の考慮漏れのうち、IDP/Viewer の実装には影響せず、外部システム（Power Automate・DataDelivery）担当者との調整で確定する事項を記録する。確定後に `docs/business` 配下へ反映するのを忘れないための備忘Issue。

**確定前にドキュメントへ反映しない。** 各項目は担当者調整の結果を受けて更新する。

### 作業内容

調整対象と、確定後に反映する想定箇所。

1. **文書登録と IDP 実行の2段呼出（K②-5／K④-4）**
   - 現状: 工程表は PA が文書IDを発行して直接 IDP を呼ぶ記載。SaaS は `POST /api/v1/documents`（登録）→ `POST /api/v1/documents/{id}/idp-runs`（実行）の2段
   - 調整: PA 担当者へ2段呼出を説明
   - 反映先: `operation-flow.md` K②-5〜6・K④-4〜6 に登録工程を追加

2. **画面登録文書（VW-08）の後続経路**
   - 現状: Viewer 画面から登録した文書は連携フォルダを経由しないため、K②-7〜8（案件ID解決〜DD保管）・K④-10（保管フォルダ移動）に接続しない。文書IDの発番方法も調整中（Viewer が採番しないことは確定）
   - 調整: 画面登録を含めた業務フローの再検討、発番方法の確定
   - 反映先: `operation-flow.md`（画面登録経路の工程追加）、`data-model.md` E01 採番マスタ備考、必要なら `out-of-scope.md` に OUT 追加

3. **DD の K05 注文文書「抽出項目JSON・IDP確信度」の供給工程**
   - 現状: 列は定義されているが、PA がどの IDP 実行結果（初回／最新）をいつ DD へ書くかの工程がない。K⑤-10／R①-10「注文DATA」の意味も未定義。SAP には抽出結果が存在しないため供給元は IDP 実行結果のみ
   - 調整: 初回結果を K④-6 直後に即時登録（見積の K②-8 と対称）か、日次バッチで `GET /api/v1/documents/{id}/idp-runs` から採用するかを決定。API 追加は不要
   - 反映先: `operation-flow.md` K④／K⑤-10／R①-10、`data-model.md` K05・K07 備考、`out-of-scope.md` OUT-07（PDF実体は日次・抽出結果は即時、等）

4. **OUT-15「再実行は実行履歴から手動で行う」の実行履歴の所在**
   - 現状: 型区分判定ログをモデルから除いたため、参照先が不明
   - 調整: Power Automate の実行履歴を指すことの確認
   - 反映先: `out-of-scope.md` OUT-15

5. **凍結日ドリフト（OUT-16）**
   - 現状: D13／D14 は「月末時点」の凍結値だが、凍結実行は翌月3日。K01 は UPDATE 可のため月末〜3日の更新が凍結値に混入する
   - 調整: 留保として許容するか、凍結対象を「月末時点の値」に限定する手段を設けるか
   - 反映先: `out-of-scope.md` OUT-16 の「PoC結果の留保」

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- [ ] 上記5項目について担当者調整の結果が確定している
- [ ] 確定内容が `docs/business` 配下（`operation-flow.md`・`data-model.md`・`out-of-scope.md`）へ反映されている
- [ ] 反映内容に IDP/Viewer の実装変更が必要なものが含まれる場合、別途 Feature Issue を起票している

### 関連情報

- #109（`docs/business` 更新）
- ADR-0003（業務キー／文書キー分離）、ADR-0005（実行ログ追記型）
- 本リポジトリの ADR は実装者の仮決定であり関係者合意済ではない

