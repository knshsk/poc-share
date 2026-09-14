title:	[Bug] build-images.sh が Viewer の OIDC authority に factone-poc-keycloak を直書きしており、環境名 poc 以外で誤った値が焼き込まれる
state:	CLOSED
labels:	bug
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
number:	143
--
### 事象

`scripts/build-images.sh` は Viewer のビルド引数 `VITE_OIDC_AUTHORITY` を `https://factone-poc-keycloak.<Environment 既定ドメイン>/realms/factone` と組み立てており、ホスト名の `poc` が直書きされている。`bicep/main.bicep` の `environmentName` を `poc` 以外にすると、Keycloak Container App の実際のホスト名（`factone-<environmentName>-keycloak.<既定ドメイン>`）と一致しない値が Viewer に焼き込まれる。

### 再現手順

1. `bicep/main.bicepparam` の `environmentName` を `dev` にする
2. `./scripts/deploy-infra.sh` でコアインフラをデプロイする
3. `./scripts/build-images.sh` を実行する
4. 出力される `Viewer OIDC authority:` を確認する

### 期待する動作

`https://factone-dev-keycloak.<既定ドメイン>/realms/factone` が焼き込まれる。

### 実際の動作

`https://factone-poc-keycloak.<既定ドメイン>/realms/factone` が焼き込まれ、Viewer のログインが Keycloak に到達しない。

### 対象コンポーネント（複数選択可）

スクリプト（`scripts` 配下）

### 環境

Azure（`scripts/build-images.sh` 経由の ACR ビルド）

### 備考

`docs/azure/README.md`（#141）に現状として記載済み。修正時は同文書の「トポロジ」節の記述も更新する。あわせて `VITE_OIDC_CLIENT_ID` をビルド引数で渡していない点（Viewer 既定値 `factone-viewer` に依存）も同スクリプトで扱うか検討する。
