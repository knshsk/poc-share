title:	[Feature] API契約の型共有パイプライン（OpenAPIエクスポート・TS型生成・差分チェック）
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
number:	13
--
### 目的

Pydanticモデルを単一情報源とし、フロントエンドへ型安全にAPI契約を共有する生成パイプラインを確立する。生成物のドリフトを pre-commit で検出し、契約と実装の乖離を防ぐ。

### 変更内容

- `api/scripts/export_openapi.py` — FastAPIアプリのOpenAPIスキーマをJSON出力するスクリプト
- `docs/api/openapi.json` — エクスポート固定物（外部向け契約レビュー・共有用の正）
- `viewer/src/api/schema.d.ts` — openapi-typescript による生成TS型（コミット対象）
- viewer に `openapi-typescript`（dev）・`openapi-fetch` を追加
- `npm run generate:api` — エクスポート→TS型生成の一括入口
- pre-commit フック追加 — 再生成し生成物に差分があれば失敗
- `docs/develop-guide/` へ生成手順を追記

### 完了条件

- [ ] `npm run generate:api` で `docs/api/openapi.json` と `viewer/src/api/schema.d.ts` が再生成される
- [ ] 生成物が最新の状態で `pre-commit run --all-files` が通過する
- [ ] APIスキーマ変更後に生成物を更新しないと pre-commit が失敗する
- [ ] 生成手順がドキュメントに記載されている

### 関連情報

- 関連要件: IF-01 / IF-02（実契約の定義自体は本Issueのスコープ外。現時点の契約は `/health` のみ）

## Comments

