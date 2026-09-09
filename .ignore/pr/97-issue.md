title:	[Feature] RFC 9457準拠のAPI共通エラーレスポンス（Problem Details）を導入する
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
number:	97
--
### 目的

外部呼出元（Power Automate 等）がエラー種別を `type` で機械判別できるよう、API のエラー応答を RFC 9457 (Problem Details for HTTP APIs) に準拠した共通形式へ統一する。あわせて OpenAPI に全 4xx/5xx 応答を明記し、外部向け契約として固定する。

### 変更内容

- `api/app/core/problem.py` を新設し、`Problem` モデル・`ProblemException`・例外ハンドラ（HTTPException / RequestValidationError / 未捕捉例外）・OpenAPI 用ヘルパを集約する
- 全エラー応答を `Content-Type: application/problem+json`、本文 `{type, title, status, detail}`（422 のみ `errors` 拡張）にする。`type` は `/errors/<code>` 形式の相対 URI
- 既存の `raise HTTPException(...)` を発生箇所ごとの固有コード付き `ProblemException` に置換する
- ルータ／ルートの `responses=` でエラー応答を OpenAPI に載せ、`docs/api/openapi.json` と `viewer/src/api/schema.d.ts` を再生成する
- ADR を起票し、`docs/api/README.md` にエラー応答の節を追加する

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] 全エラー応答が `application/problem+json` で `type/title/status/detail` を返す
- [ ] `/openapi.json` に `Problem` スキーマがあり、`HTTPValidationError` が残っていない
- [ ] 異常系テスト（`api/tests/test_problem.py`）を追加し `uv run pytest` が全通過する
- [ ] `npm run generate:api` の生成物が最新で pre-commit が通過する
- [ ] ADR（`docs/adr/0016-rfc9457-problem-details.md`）を起票する

### 関連情報

- RFC 9457: https://www.rfc-editor.org/rfc/rfc9457
