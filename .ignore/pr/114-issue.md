title:	[Feature] API: IDP 解析結果 API の一覧サマリ化・詳細・latest 分割
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
number:	114
--
### 目的

`GET /api/v1/documents/{id}/idp-runs` は全実行結果を項目値・テーブル込みで返すため、実行回数に比例して応答が肥大する。一覧（サマリ）と詳細を分け、消費側（WalkMe / Power Automate）が「最新の結果」を 1 回で取得できるようにする。

### 変更内容

- `GET /api/v1/documents/{id}/idp-runs`: 応答を `IdpRunSummary`（`run_id` / `document_id` / `run_type` / `status` / `executed_at` / `base_run_id` / `corrected_by` / `di_api_version` / `di_model_id` / `error_info`）の配列へ縮小する。順序は現状維持（`executed_at` 昇順）
- 新設 `GET /api/v1/documents/{id}/idp-runs/{run_id}`: 詳細（`IdpRunResponse`）。他文書の run_id は 404 `idp-run-not-found`
- 新設 `GET /api/v1/documents/{id}/idp-runs/latest`: 修正を含む最新の `succeeded` 行（`executed_at` 降順・同時刻は `id` 降順）。任意クエリ `run_type=initial|rerun|corrected` で絞り込み。該当なしは 404。`{run_id}` より先にルートを宣言する
- 一覧の縮小は ADR-0017 上の破壊的変更だが、外部利用者未接続のため v1 内で実施する。ADR-0017 に「外部利用者が接続するまでは v1 内の破壊的変更を許容（本件で適用）」を追記し、`docs/api/README.md` にも記録する
- Viewer `WorkPanel` を「一覧取得 → 最新を選択 → 詳細取得」「履歴選択で詳細取得」に追従させる（一覧項目から結果を読む現行実装が壊れるため同 PR で対応）。表示は現状のまま
- ドキュメント: `docs/data-model/idp-runs.md`（API 一覧）、`docs/api/README.md`（エラーコード・記録）
- OpenAPI / TS 型の再生成

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 一覧応答に `raw_fields` / `normalized_fields` / `confidence` / `tables` / `pattern_matches` が含まれない（テスト添付）
- 詳細の 200 / 他文書の run_id の 404、latest の区分絞り込みと失敗のみのときの 404 がテストで確認されている
- Viewer の読取結果・実行履歴が従来どおり表示される（`npm run test` 更新）
- ADR-0017 の追記と設計ドキュメントの更新が含まれる
- `uv run pytest` / `npm run test` / pre-commit 通過

### 関連情報

- 親: #112
- ADR-0017（API バージョニング）

