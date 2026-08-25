title:	[Feature] 品質ゲート整備（pre-commit・lint・型検査の入口一元化）
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
number:	11
--
### 目的

コミット時の品質チェック（lint・format・型検査）を自動化し、将来のCI移行を容易にするため品質コマンドの入口を一元化する。

### 変更内容

- リポジトリルートに `.pre-commit-config.yaml` を新規作成
  - API: ruff check（--fix）・ruff format、mypy（local hook・`uv run mypy`）
  - Viewer: eslint・prettier（local hook・`npm run` 経由）
- API側: dev依存へ mypy 追加、`pyproject.toml` に ruff・mypy 設定（mypy は strict）
- Viewer側: eslint（flat config・Vue/TypeScript対応）・prettier 導入、`npm run lint`・`npm run format:check` スクリプト追加
- 入口一元化: lint・format・型検査は `pre-commit run --all-files` に集約（将来CIも同一入口）
- `docs/develop-guide/quality.md` — pre-commit セットアップ・実行手順
- `docs/develop-guide/README.md` — ドキュメント一覧の更新（api.md・viewer.md の掲載漏れ修正含む）

### 完了条件

- [ ] `pre-commit run --all-files` が既存コード全体で通過する
- [ ] ruff・mypy・eslint・prettier が pre-commit 経由で実行される
- [ ] `pre-commit install` でコミット時に自動実行される
- [ ] 手順が `docs/develop-guide/quality.md` に記載され、README 一覧が最新化されている

### 関連情報

- スコープ外: CI、TS型生成の差分チェック、シークレット検出フック

## Comments

