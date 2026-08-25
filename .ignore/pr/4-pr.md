title:	feat: ローカル開発スタック整備（#3）
state:	MERGED
author:	shiro-ino (しろいの)
labels:	
assignees:	
reviewers:	
projects:	
milestone:	
number:	4
url:	https://github.com/shiro-ino/factone-idp-viewer/pull/4
additions:	288
deletions:	1
auto-merge:	disabled
--
# 概要

ローカル開発で必要となるKeycloak・PostgreSQLをコンテナで再現可能にします。

## 関連Issue

Closes #3

## 変更内容

- `compose.yaml` を追加（PostgreSQL 17 + Keycloak 26.7。Podman Compose実行前提）
  - PostgreSQLのメジャーはAzure PostgreSQL Flexible Serverの最新対応版（17）に整合
  - Keycloakはヘルスチェック通過後に起動し、`start-dev --import-realm` でrealm定義を自動投入
- `scripts/local-stack/init-keycloak-db.sh` — 初回起動時にKeycloak用DB・ユーザーを作成（アプリ用DBと同一サーバ内別DB構成をAzure構成と相似に）
- `keycloak/realm/factone.json` — realm定義初版（最小構成・シークレット非含有。クライアント・ユーザーは認証統合時に拡充）
- `keycloak/README.md` — config-as-code暫定運用ルール（realm JSONを正とする、シークレット非含有）
- `.env.example` 追加、`.gitignore` に `.env` 追加
- `.gitattributes` で `*.sh` のLF強制（Windows環境のCRLF化でコンテナ内実行が壊れる問題の予防）
- `docs/develop-guide/local-stack.md` — 起動・停止・初期化・トラブルシュート手順

## 確認事項

### 共通

- [x] ブランチ名が規約に従っている（`feature/<issue番号>-<簡易説明>` または `fix/<issue番号>`）
- [ ] pre-commit のチェックがすべて通過している（※pre-commit は未導入のため対象外）
- [x] 影響する設計ドキュメント（`docs/` 配下）を更新した、または更新不要である

### テスト

- [ ] `uv run pytest` / `npm run test` をローカルで実行し、すべて通過している（※テスト基盤は未導入のため対象外）
- [x] 重点テスト領域に変更がある場合、対応するテストを追加・更新した（変更なし）

### API契約

- [x] Pydanticモデルに変更がある場合、TS型を再生成し差分をコミットした（変更なし）
- [x] 外部公開契約（Power Automate向けIF）に影響する場合、`docs/api` のOpenAPIエクスポートを更新した（影響なし）

### アーキテクチャ

- [x] アーキテクチャ上の意思決定を含む場合、ADR（`docs/adr/`）を作成・更新した（新規決定なし。Keycloak構成管理・ローカルランタイムの既決事項のADR化は別途）

## 動作確認方法

WSL2+Podman環境で以下を実行してください（Windowsホストでは `docker compose config` による構文検証まで実施済み）。

1. `cp .env.example .env` し、`changeme-` の値を書き換える
2. `podman compose up -d`
3. `podman compose ps` で両サービスがhealthy/runningであること
4. http://localhost:8080 の管理コンソールにログインし、realm `factone` が存在すること
5. `podman compose down -v` で後片付け

## 備考

- realm設計は未確定のため、realm定義は最小構成に留めています。設計ドキュメント化は確定後に `docs/architecture/` へ。
- Keycloakの管理者はブートストラップ管理者（`KC_BOOTSTRAP_ADMIN_*`）です。恒久管理者の整備は認証統合時に検討します。

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Comments

author:	shiro-ino
association:	owner
edited:	false
status:	none
--
追加コミット: WSL開発環境構築手順（`docs/develop-guide/wsl-setup.md`）を追加しました。

- 分割方針: 実施頻度で分割（マシン1回きりの環境構築=wsl-setup / 日常操作=local-stack）
- `local-stack.md` の前提条件を wsl-setup.md へのリンクに差替え
- `develop-guide/README.md` に読み順付きのドキュメント一覧を追加

レビュー観点: プロキシ・証明書設定手順が社内環境の実態と合っているかを確認してください。
--
