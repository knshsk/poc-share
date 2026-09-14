title:	[Feature] DI全文に対する正規表現パターン抽出をIDP実行に追加する
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
number:	107
--
### 目的

Query Fields では取得しづらい項目（採番規則のある見積番号、適格請求書発行事業者登録番号 T コード等）を補うため、Document Intelligence の全文テキストに正規表現でパターンマッチを行い、マッチした文字列を IDP 実行結果に含めて返す。PoC ではマッチ値の妥当性検証は外部システムの責務とし、SaaS 側は検出と返却のみ行う。

### 変更内容

- 文書種別設定 `document_type_configs` に `patterns` 配列（`{key, label, regex}`）を追加し、設定 API で管理する。`regex` は登録時に `re.compile` で検証（最大 512 文字）
- `AnalysisResult` に DI 応答 `content`（全文テキスト）を追加する
- IDP 実行時に各パターンを `re.finditer` で全文へ適用し、出現順・重複ありの全一致を `idp_runs.pattern_matches` JSONB に保存して応答に含める。実行時点の `label` / `regex` も同梱する
- 既存シード 2 種別（`purchase_order` / `quotation`）に `TCode`（`T\d{13}`）を投入する
- Viewer は変更しない（`schema.d.ts` 再生成のみ）
- `docs/data-model/` の該当ドキュメントを更新する

### 対象コンポーネント（複数選択可）

- API（`api` 配下）
- ドキュメント（`docs` 配下）

### 完了条件

- [ ] 設定 API で `patterns` を登録・取得でき、不正な正規表現は 422 になる
- [ ] IDP 実行応答と履歴に `pattern_matches` が含まれ、複数一致は出現順・重複ありで返る
- [ ] DI 失敗時の `pattern_matches` は null
- [ ] `openapi.json` / `schema.d.ts` を再生成し pre-commit 通過
- [ ] `docs/data-model/document-type-configs.md` / `idp-runs.md` を更新

### 関連情報

- ADR-0014（テナント別文書種別設定。本件はその拡張で新規 ADR は起票しない）
- ADR-0005（追記型ログ）
