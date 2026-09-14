title:	[Feature] 人手修正の並行更新（排他制御）の方針決定と実装
state:	OPEN
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
number:	129
--
### 目的

人手修正（`POST …/idp-runs/{run_id}/corrections`）には排他制御が無く、同一 base への並行修正は両方追記され `latest` は新しい方を返す（PoC 制限、ADR-0018 / 設計 §10）。本番運用前に、並行修正の扱いを決める必要がある。

### 変更内容

方針を決めてから実装する。候補:

- A. `idp_runs.base_run_id` に部分一意制約（`corrected` 行のみ）を付け、同一 base への 2 件目を 409 `idp-run-already-corrected` で拒否する。Viewer は 409 時に最新を再取得して案内する
- B. 楽観ロック: リクエストに `expected_latest_run_id` を持たせ、文書の最新行と不一致なら 409
- C. 現状維持（最新が勝つ）を正式仕様とし、Viewer の履歴で並行修正が見えることを担保する

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 方針が決まり ADR-0018 に追記（または新 ADR）
- A / B の場合: マイグレーション・API テスト（並行修正の 409）・Viewer のエラー表示とテスト
- `docs/data-model/idp-runs.md` / `docs/api/README.md` 更新

### 関連情報

- ADR-0018、設計 §5.2 / §10
- #115（corrections API）

