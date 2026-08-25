title:	[Feature] アプリデプロイ基盤（Container Apps・ACR・Containerfile・同居配信）
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
number:	31
--
### 目的

APIとViewerをAzureへデプロイ可能にする。コンテナ化（マルチステージ）、ACRクラウドビルド、Container Apps（API+Keycloak相乗り）のBicep、フロント同居配信の実装を整備する。

### 変更内容

コンテナ化:

- `api/Containerfile` — マルチステージ（node で viewer ビルド → Python実行イメージへ成果物コピー。コンテキスト=リポジトリルート）。ViewerのOIDC設定（VITE_OIDC_AUTHORITY等）はビルド引数で焼き込み
- `keycloak/Containerfile` — 公式イメージベース+realm JSON焼き込み（--import-realm用）+`kc.sh build` 済み最適化（起動時リビルド回避）
- API: 静的配信追加（`viewer/dist` 存在時のみマウント・SPAフォールバック。API経路優先）

Bicep:

- `acr.bicep` / `container-apps-env.bicep`（+Log Analytics）/ `identities.bicep`（ユーザー割当マネージドID。ACRプルのロール付与順序の制約によりシステム割当から変更）/ `app-roles.bicep`（Blob Data Contributor・Cognitive Services User・Key Vault Secrets User・AcrPull）/ `api-app.bicep`/ `keycloak-app.bicep`（共用PostgreSQL内別DB）
- main.bicep へ統合（イメージ未指定時はアプリ層スキップ。revisionSuffixで同一タグでも新リビジョン・再プルを強制）

スクリプト・ドキュメント:

- `scripts/build-images.sh`（az acr build。Keycloak FQDNを解決しViewerビルド引数へ注入）/ `scripts/deploy-apps.sh`
- `docs/develop-guide/deploy.md` 更新

### 完了条件

- [ ] `az bicep build` がエラー・警告なしで通過する
- [ ] 静的配信（SPAフォールバック・API経路優先）がテストで担保されている
- [ ] ビルド・デプロイ手順が deploy.md に記載されている
- [ ] pre-commit・pytest・Vitest すべて通過

### 関連情報

- 決定済み方針: Container Apps同居配信 / az acr build / Keycloak相乗り・realm config-as-code
- 割切り: KeycloakのDB接続はPoC限定で管理者ユーザー使用（本番化時に専用ユーザー化）
- 実デプロイ検証は az login 可能な環境での手動実施


## Comments

