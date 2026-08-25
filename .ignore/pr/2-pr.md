title:	feat: リポジトリスキャフォールディング（#1）
state:	MERGED
author:	shiro-ino (しろいの)
labels:	
assignees:	
reviewers:	
projects:	
milestone:	
number:	2
url:	https://github.com/shiro-ino/factone-idp-viewer/pull/2
additions:	232
deletions:	0
auto-merge:	disabled
--
# 概要

開発初期基盤として、リポジトリ規約類・ドキュメント骨格・ディレクトリ雛形を整備します。

## 関連Issue

Closes #1

## 変更内容

- `.github/` テンプレート一式を追加（PULL_REQUEST_TEMPLATE.md、ISSUE_TEMPLATE/feature.yml・bug.yml・config.yml）
- `docs/` 骨格を整備（adr・api・architecture・azure・data-model・develop-guide・viewer の各サブディレクトリにREADME.md、ADRテンプレート）
- `docs/db` を `docs/data-model` へリネーム（開発方針のディレクトリ構成案に整合）
- ディレクトリ雛形として `api/tests/fixtures`・`bicep`・`keycloak`・`samples`・`scripts`・`viewer` に `.gitkeep` を配置

## 確認事項

### 共通

- [x] ブランチ名が規約に従っている（`feature/<issue番号>-<簡易説明>` または `fix/<issue番号>`）
- [ ] pre-commit のチェックがすべて通過している（※pre-commit は別ブランチで導入予定のため対象外）
- [x] 影響する設計ドキュメント（`docs/` 配下）を更新した、または更新不要である

### テスト

- [ ] `uv run pytest` / `npm run test` をローカルで実行し、すべて通過している（※テスト基盤は別ブランチでで導入予定のため対象外）
- [x] 重点テスト領域に変更がある場合、対応するテストを追加・更新した（変更なし）

### API契約

- [x] Pydanticモデルに変更がある場合、TS型を再生成し差分をコミットした（変更なし）
- [x] 外部公開契約（Power Automate向けIF）に影響する場合、`docs/api` のOpenAPIエクスポートを更新した（影響なし）

### アーキテクチャ

- [x] アーキテクチャ上の意思決定を含む場合、ADR（`docs/adr/`）を作成・更新した（新規決定なし。既存決定のADR化は別途）

## 動作確認方法

- PR差分でファイル構成・README内容・テンプレート内容を確認してください。

## 備考

- `compose.yaml` は別スコープのため含めていません。
- レビュー対象は規約類（Issue/PRテンプレート・ディレクトリ構成・docs骨格）です。

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Comments

