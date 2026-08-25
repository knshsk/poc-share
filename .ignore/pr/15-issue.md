title:	[Feature] コアインフラのBicep整備（DI・Blob・PostgreSQL・Key Vault）
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
number:	15
--
### 目的

PoC環境のコアAzureリソース（Document Intelligence・Blob Storage・PostgreSQL・Key Vault）をIaC（Bicep）で定義し、再現可能なデプロイ手段を確立する。

### 変更内容

- `bicep/main.bicep` — リソースグループスコープのエントリポイント（モジュール呼出）
- `bicep/modules/` — サービス単位のモジュール分離（将来のPrivate Endpoint後付けを考慮した構造）
  - `document-intelligence.bicep`（S0）
  - `storage.bicep`（Blob。匿名アクセス無効・TLS強制）
  - `postgresql.bicep`（Flexible Server B1ms・ファイアウォール規則）
  - `keyvault.bicep`（RBAC認可）
- `bicep/main.bicepparam` — パラメータ定義（環境名・リージョン等）
- `scripts/deploy-infra.sh` — リソースグループ作成＋デプロイのbashラッパー
- `docs/develop-guide/deploy.md` — 前提条件・使い方・トラブルシュート（手順の正はスクリプト側）

### 完了条件

- [ ] `az bicep build` がエラー・警告なしで通過する
- [ ] `scripts/deploy-infra.sh` によるデプロイ手順が `docs/develop-guide/deploy.md` に記載されている
- [ ] PostgreSQL管理者パスワードが `@secure()` パラメータで扱われ、Key Vault に格納される

### 関連情報

- スコープ外: Container Apps・ACR・Keycloak配置、マネージドID割当、VNet/Private Endpoint

## Comments

