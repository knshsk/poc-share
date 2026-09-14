title:	[Feature] API: IDP 読取結果の人手修正の保存
state:	CLOSED
labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#112
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	115
--
### 目的

Viewer で修正した読取結果を、実行ログと同じ形で保存・取得できるようにする。修正は追記型（ADR-0005）とし、機械実行と区別して履歴に残す。

### 変更内容

- `idp_runs.run_type` に `corrected` を追加（CHECK 制約更新）。列追加: `base_run_id`（uuid、自己参照 FK）/ `corrected_by`（varchar 255）/ `corrections`（jsonb）。`corrected` 行のみ非 NULL（CHECK 制約）
- 新設 `POST /api/v1/documents/{id}/idp-runs/{run_id}/corrections`: `{run_id}` を base に `corrected` 行を追記し 201 で `IdpRunResponse` を返す
  - ボディは差分形式 `{"fields": {key: string|null}, "table_cells": [{table_index, row_index, column_index, content}]}`。各値 2000 文字上限。両方空・重複セルは 422 `validation-error`
  - 検証: base が `failed` → 409 `idp-run-not-correctable`、base に無いキー / セル座標 → 422 `unknown-correction-target`
  - `corrected` 行の値: `raw_fields` は base に差分を上書き、`normalized_fields` は変更キーのみ現在の文書種別設定の型で再正規化（設定に無いキーは string）、`confidence` は変更キーを null、`tables` は構造を維持して `content` のみ差し替え、`field_labels` / `pattern_matches` / `field_regions` / `di_api_version` / `di_model_id` / `raw_response_ref` は base からコピー、`corrections` に受け取ったボディをそのまま保存、`corrected_by` は JWT の `preferred_username`
  - `corrected` 行を base にした再修正（連鎖）を許す。排他制御は行わない（PoC 制限として明記）
- `IdpRunResponse` / `IdpRunSummary` に `base_run_id` / `corrected_by`、`IdpRunResponse` に `corrections` を追加
- ドキュメント: `docs/data-model/idp-runs.md`、`docs/api/README.md`（エラーコード）、ADR-0018（人手修正を `idp_runs` の `corrected` 行として追記保存。却下案: 別テーブル・上書き更新）、`docs/requirements/viewer.md` に VW-09「読取結果の修正・確定」を追加
- OpenAPI / TS 型の再生成

### 対象コンポーネント（複数選択可）

API（`api` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- フィールド / セルのマージ、変更キーの確信度 null と未変更キーの維持、現行設定型での再正規化、各列のコピー、`corrections` の保存、連鎖修正がテストで確認されている
- 異常系（両方空・重複セル・未知キー / セル・失敗 base・他文書）が仕様どおりの status / type を返す（テスト添付）
- `latest` が修正行を返し、`?run_type=corrected` で絞り込める
- ADR-0018・VW-09・設計ドキュメントの更新が含まれる
- `uv run pytest` / pre-commit 通過、TS 型の差分チェック通過

### 関連情報

- 親: #112
- ADR-0005（追記型・生値と正規化値の分離）

