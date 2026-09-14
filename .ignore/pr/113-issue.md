title:	[Feature] API: DI 生応答の Blob 保存と実行時情報の記録
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
number:	113
--
### 目的

Document Intelligence の応答を `tables` / `fields` 以外も含めてそのまま残し、将来の機能拡張（座標利用など）とカスタムモデル学習に備える。あわせて実行時の API バージョン・モデル ID を記録し、DI 側の変更に追従できるようにする。

### 変更内容

- `idp_runs` に列追加: `di_api_version`（varchar 20）/ `di_model_id`（varchar 100）/ `raw_response_ref`（varchar 1024）/ `field_regions`（jsonb）。すべて NULL 可、失敗実行は NULL
- 生応答の保存: DI SDK の `begin_analyze_document(cls=...)` コールバックで最終ポーリング GET の応答本文（bytes、ラッパ込み）を取得し、Blob `{tenant_id}/{document_id}/idp-runs/{run_id}.json` へそのまま保存する。再シリアライズしない。Blob 保存失敗は 500 とし実行ログは記録しない
- `field_regions`: `documents[0].fields[key].boundingRegions[0]` から `{key: {"page_number", "polygon"} | null}` を保存する（相互ハイライトのデータ源）
- DI アダプタ: `QueryFieldResult` に `page_number` / `polygon`、`AnalysisResult` に `api_version` / `model_id` / `raw_response` を追加。テストのフェイクも対応
- 新設 `GET /api/v1/documents/{document_id}/idp-runs/{run_id}/raw-response`: Blob の bytes を `application/json` で返す。参照が無い行は 404 `raw-response-not-found`
- `IdpRunResponse` に `di_api_version` / `di_model_id` / `field_regions` を追加（`raw_response_ref` は非公開）
- ドキュメント: `docs/data-model/idp-runs.md`、`docs/api/README.md`（エラーコード）、ADR-0019（DI 生応答を Blob にバイト保存。却下案: DB JSONB 列）
- OpenAPI / TS 型の再生成

### 対象コンポーネント（複数選択可）

API（`api` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 成功実行で生応答が上記パスにバイト一致で保存され、`di_api_version` / `di_model_id` / `field_regions` が記録される。失敗実行は 4 列とも NULL
- `raw-response` が保存 bytes を返し、失敗実行は 404 になる（テスト添付）
- 領域あり / 領域なし / 値なしの `field_regions` がテストで確認されている
- ADR-0019 と設計ドキュメントの更新が含まれる
- `uv run pytest` / pre-commit 通過、TS 型の差分チェック通過

### 関連情報

- 親: #112
- ADR-0004（外部システム境界アダプタ）、ADR-0010（Blob の API プロキシ）

