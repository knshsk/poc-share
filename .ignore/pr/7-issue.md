title:	[Feature] APIスケルトン（FastAPI起動・health・pytest・設定読込）
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
number:	7
--
### 目的

API開発の土台を整備する。以降のAPI実装（認証連携・文書処理等）が乗る最小の骨格を先に確立し、起動・テスト・設定読込の各基盤を確定させる。

### 変更内容

- `api/pyproject.toml` を新規作成（uv管理、Python 3.14、依存: fastapi / uvicorn / pydantic-settings、dev: pytest / httpx）
- `api/app/main.py` — `create_app()` ファクトリと `GET /health` エンドポイント（200 / `{"status": "ok"}`。DB接続チェックは含めない）
- `api/app/config.py` — pydantic-settings による設定読込骨格（`.env` 読込、環境変数プレフィックス `FACTONE_`）
- `api/tests/` — pytest基盤（conftest.py + healthエンドポイントのテスト）
- `docs/develop-guide/api.md` — 起動・テスト実行手順（最小）
- `.gitignore` へ Python 系エントリ追記（`.venv/`・`__pycache__/` 等）

### 完了条件

- [ ] `uv run uvicorn` でAPIが起動し `GET /health` が 200 を返す
- [ ] `uv run pytest` が通る（healthエンドポイントのテスト含む）
- [ ] 設定読込骨格が環境変数・`.env` から値を読める
- [ ] 起動・テスト手順が `docs/develop-guide/api.md` に記載されている

### 関連情報

- スコープ外: DB接続、認証（JWT検証）、lint/型検査設定、OpenAPI型生成


## Comments

