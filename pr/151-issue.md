title:	[Feature] IDP 実行結果応答を外部向け単一スキーマに再編し verbose パラメータで Viewer 向け情報を追加する
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
number:	151
--
### 目的

外部連携システムが IDP 実行結果を読みやすい形で取得できるようにする。読取領域・セル座標・人手差分は Viewer 専用の情報であり、外部向け応答には不要。エンドポイントは増やさず、`verbose` クエリパラメータで同一スキーマに付加情報を追加する。

### 変更内容

- `IdpRunResponse` の `raw_fields` / `normalized_fields` / `confidence` / `field_labels` / `field_regions` を `fields[key] = {value, normalized_value, confidence}` に統合する
- `verbose=true`（既定 false）のときだけ `fields[key].label` / `fields[key].region` / `tables[].cells[].polygon` / `corrections` を返す。`IdpRunResponse` を返す 4 エンドポイント（POST 実行 / GET latest / GET {run_id} / POST corrections）すべてに付ける
- `tables[].cells[]` に `row_span` / `column_span` を追加する（DI から抽出。既存行は応答で 1 に補完）
- `pattern_matches` とセルの `content` は現行維持
- Viewer は `verbose=true` で呼び、新形状に追従する
- docs（api/README の破壊的変更記録、architecture/api、data-model/idp-runs、viewer 文書）、ADR-0022、OpenAPI 再生成

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] 非 verbose 応答に `label` / `region` / `polygon` / `corrections` キーが無く、verbose 応答に全部ある（API テスト）
- [ ] 4 エンドポイントすべてで `verbose` が効く（API テスト）
- [ ] `row_span` / `column_span` が DI から抽出され、未保存の既存行は 1 で返る（API テスト）
- [ ] Viewer のハイライト・読取結果タブ・修正確定が新形状で動く（Vitest）
- [ ] `docs/` と ADR-0022 を更新し、`openapi.json` / `schema.d.ts` を再生成した
- [ ] pre-commit・`uv run pytest`・`npm run test`・`npm run build` が通る

### 関連情報

- ADR-0017（バージョニング。外部未接続期間の v1 内破壊的変更）、ADR-0018（corrected 行）、#114（一覧のサマリ化）
