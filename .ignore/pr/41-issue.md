title:	[Bug] Azureデプロイ環境でIDP実行がDocument Intelligenceエラーになる（FACTONE_DI_ENDPOINT未設定）
state:	CLOSED
author:	shiro-ino (しろいの)
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
number:	41
--
### 事象

Azureにデプロイした環境でIDP実行APIを呼び出すと、Document Intelligenceの実行エラー（502）になる。

原因調査の結果、Container App（`bicep/modules/api-app.bicep`）の環境変数に `FACTONE_DI_ENDPOINT` が設定されておらず、APIが `UnconfiguredAnalyzer`（接続先未設定のプレースホルダ）で起動しているため。

- `api/app/config.py:45` — `di_endpoint` の既定値が `""`
- `api/app/di.py:88-93` — endpoint が空のため `UnconfiguredAnalyzer` が選択される
- `api/app/di.py:38` — `DiError("Document Intelligence endpoint is not configured")` を送出
- `api/app/idp_runs.py:89-94` — `DiError` を502に変換

`main.bicep` はDIエンドポイントをoutput（`documentIntelligenceEndpoint`）として出力するのみで、`apiApp` モジュールへ渡していない。`scripts/deploy-infra.sh` 側でも注入していない。

なお認証・ロール周りは設定済みで問題なし:

- `bicep/modules/document-intelligence.bicep:16` — `customSubDomainName` 設定済（Entra ID認証の前提条件）
- `bicep/modules/app-roles.bicep:52-63` — APIのマネージドIDにCognitive Services Userロール付与済
- `bicep/modules/api-app.bicep:81` — `AZURE_CLIENT_ID` 設定済
- `api/app/di.py:47-58` — api_key未設定時のManaged ID認証（`DefaultAzureCredential`）実装済

### 再現手順

1. `scripts/deploy-infra.sh` でAzureへインフラ＋アプリ層をデプロイ
2. Viewerにログインし、文書に対してIDP実行を呼び出す
3. 実行が失敗する

### 期待する動作

Azure環境でIDP実行APIがDocument Intelligenceを呼び出し、抽出結果を返す。

### 実際の動作

IDP実行APIが502を返す。

```
502 Document analysis failed
```

APIログ上の例外:

```
DiError: Document Intelligence endpoint is not configured
```

### 対象コンポーネント

Bicep（インフラ）

### 環境

Azure（Container Apps上のAPI）。ローカルでは `.env` の `FACTONE_DI_ENDPOINT` 設定で動作するため再現しない。

### 修正方針（案）

- `bicep/modules/api-app.bicep` に param `diEndpoint` を追加し、env に `{ name: 'FACTONE_DI_ENDPOINT', value: diEndpoint }` を追加
- `bicep/main.bicep` の `apiApp` モジュール呼出に `diEndpoint: documentIntelligence.outputs.endpoint` を追加
- APIキーは不要（Managed ID認証に自動フォールバック）

## Comments

