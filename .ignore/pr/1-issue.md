title:	[Feature] リポジトリスキャフォールディング（.githubテンプレ一式・docs骨格・ディレクトリ雛形）
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
number:	1
--
## 目的

開発初期基盤としてリポジトリ規約類・ドキュメント骨格・ディレクトリ雛形を整備する。

## 変更内容

- `.github/` テンプレート一式のコミット（PULL_REQUEST_TEMPLATE.md、ISSUE_TEMPLATE/feature.yml・bug.yml・config.yml）
- `docs/` 骨格の整備（adr・api・architecture・azure・data-model・develop-guide・viewer。各サブディレクトリに目的説明のREADME.md）
- `docs/db` を `docs/data-model` へリネーム（開発方針のディレクトリ構成案に整合）
- ADRテンプレート（`docs/adr/0000-adr-template.md`）のコミット
- ディレクトリ雛形: `api/tests/fixtures`・`bicep`・`keycloak`・`samples`・`scripts`・`viewer` に `.gitkeep` 配置

## 完了条件

- 上記ファイル群がPR経由でmainへマージされている
- `compose.yaml` は含めない（S2スコープ）

## 関連情報

- なし

## Comments

