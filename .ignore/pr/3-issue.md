title:	[Feature] ローカル開発スタック整備（compose.yaml・Keycloak realm初版・起動手順）
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
number:	3
--
## 目的

ローカル開発で必要となる認証基盤（Keycloak）とデータベース（PostgreSQL）を、コンテナで再現可能な形で整備する。

## 変更内容

- `compose.yaml` を追加（PostgreSQL + Keycloak。Podman Compose実行前提）
  - PostgreSQLはinitスクリプトでKeycloak用DB・ユーザーを追加作成（同一サーバ内別DB構成をローカルでも再現）
  - Keycloakは公式イメージで `start-dev --import-realm` により realm定義を自動投入
- `keycloak/realm/` に realm定義JSON初版を追加（最小構成。クライアント・ユーザー定義は認証統合時に拡充）
- `keycloak/README.md` に構成管理の暫定運用を記載（realm JSONを正とする、シークレット非含有ルール）
- `.env.example` を追加し、`.gitignore` に `.env` を追加
- `docs/develop-guide/local-stack.md` に起動手順（最小）を記載

## 完了条件

- `podman compose up -d` でPostgreSQL・Keycloakが起動し、realmが投入されること
- realm JSONにシークレットが含まれていないこと
- 起動手順ドキュメントが存在すること

## 関連情報

- realm構成は未確定のため、realm設計ドキュメントは対象外（暫定運用のみ）

## Comments

