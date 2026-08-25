title:	[Feature] 文書取込基盤（文書メタデータモデル・Blob保存・文書登録API）
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
number:	19
--
### 目的

垂直スライスの起点として、文書メタデータモデル・Blob保存・文書登録APIを整備する。以降のIDP実行・Viewer表示実装が乗る文書管理の土台。

### 変更内容

- `documents` テーブル（単一テーブル+種別列。文書種別の統合モデル採否が未確定のため仮置き明記）
  - `document_id`（PK・呼出側採番）、`tenant_id`（仮置き: 固定値 `default`）、`document_type`（text+CHECK制約）、`blob_ref`、`content_sha256`、受領・作成日時
- DB層: SQLAlchemy 2.0（同期）+ psycopg3 + Alembic（マイグレーションを正とし、テストも `alembic upgrade` でスキーマ構築）
- Blobアダプタ: `BlobStorage` Protocol + `AzureBlobStorage`（Managed ID）/ `LocalFileBlobStorage`（ローカル開発用）
- `POST /api/documents` — multipart（PDF+document_id+document_type）。JWT必須
  - 201: 新規 / 200: 同一内容の再送（冪等・sha256判定）/ 409: 同一IDで内容不一致 / 415: PDF以外 / 422: 不正入力
- realm: M2M連携用confidentialクライアント `factone-integration`（client credentials grant・audienceマッパー付）追加。シークレットはkcadmスクリプトで起動後設定
- `docs/data-model/documents.md` 新規作成、develop-guide更新

### 完了条件

- [ ] 文書登録の正常系・冪等再送・内容不一致・PDF以外・未認証の各ケースがテストで担保されている（実PGコンテナ接続）
- [ ] `factone-integration` のclient credentialsで取得したトークンで登録APIを呼び出せる
- [ ] realm JSON・リポジトリにシークレットが含まれない
- [ ] pre-commit・pytest・Vitest・API契約差分チェックすべて通過
- [ ] データモデルが `docs/data-model/documents.md` に記載されている

### 関連情報

- 仮置き: テナント識別（固定値）・文書種別の統合モデル（単一テーブル+種別列）

## Comments

