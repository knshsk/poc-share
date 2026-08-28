title:	[Chore] VSCode向けワークスペース設定の追加
state:	CLOSED
author:	shiro-ino (しろいの)
labels:	chore
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
number:	59
--
### 目的

VSCodeでのPython依存解決（Pylanceのimport解決）と、開発に必要な拡張機能の推奨を整備し、開発環境セットアップを容易にするため。

### 作業内容

- `.vscode/settings.json` を新規作成
  - `python.analysis.extraPaths` に `api/.venv/lib/python3.14/site-packages` と `api/.venv/lib64/python3.14/site-packages` を追加
  - `python.defaultInterpreterPath` に `api/.venv/bin/python` を設定
- `.vscode/extensions.json` を新規作成し、最低限の推奨拡張機能を定義
  - ms-python.python / charliermarsh.ruff / Vue.volar / dbaeumer.vscode-eslint / esbenp.prettier-vscode / ms-azuretools.vscode-bicep

### 対象コンポーネント（複数選択可）

その他

### 完了条件

- [ ] `.vscode/settings.json` が作成され、Python依存解決パスが設定されている
- [ ] `.vscode/extensions.json` が作成され、推奨拡張機能が定義されている
- [ ] pre-commit を通過している

### 関連情報

なし

## Comments

