title:	feat: APIスケルトン構築（#7）
state:	MERGED
author:	shiro-ino (しろいの)
labels:	
assignees:	
reviewers:	
projects:	
milestone:	
number:	8
url:	https://github.com/shiro-ino/factone-idp-viewer/pull/8
additions:	671
deletions:	1
auto-merge:	disabled
--
# 概要

API開発の土台として、uv管理のFastAPIプロジェクトを新規構築する。

## 関連Issue

Closes #7

## 変更内容

- `api/pyproject.toml` 新規作成（uv管理、Python 3.14、fastapi / uvicorn / pydantic-settings、dev: pytest / httpx2）
- `api/app/main.py` — `create_app()` ファクトリと `GET /health`（200 / `{"status": "ok"}`）
- `api/app/config.py` — pydantic-settings による設定読込骨格（環境変数プレフィックス `FACTONE_`・`.env` 読込）
- `api/tests/` — pytest基盤（health・設定読込のテスト、計4件）
- `docs/develop-guide/api.md` — セットアップ・起動・テスト手順
- `.gitignore` — Python系エントリ追記（`.venv/`・`__pycache__/` 等）

## 確認事項

### 共通

- [x] ブランチ名が規約に従っている（`feature/<issue番号>-<簡易説明>` または `fix/<issue番号>`）
- [ ] pre-commit のチェックがすべて通過している（pre-commit未導入のため対象外）
- [x] 影響する設計ドキュメント（`docs/` 配下）を更新した、または更新不要である

### テスト

- [x] `uv run pytest` / `npm run test` をローカルで実行し、すべて通過している（pytest 4件パス・警告なし）
- [x] 重点テスト領域（閲覧トークン、イベント記録、DI応答パース、座標変換、異常系）に変更がある場合、対応するテストを追加・更新した（該当変更なし）

### API契約

- [x] Pydanticモデルに変更がある場合、TS型を再生成し差分をコミットした（生成パイプライン未導入のため対象外）
- [x] 外部公開契約（Power Automate向けIF）に影響する場合、`docs/api` のOpenAPIエクスポートを更新した（該当変更なし）

### アーキテクチャ

- [x] アーキテクチャ上の意思決定を含む場合、ADR（`docs/adr/`）を作成・更新した（既決事項の実装のみ。新規決定なし）

## 動作確認方法

```
cd api
uv sync
uv run pytest
uv run uvicorn --factory app.main:create_app
```

`http://127.0.0.1:8000/health` にアクセスし `{"status": "ok"}` が返ることを確認。

## 備考

- テストクライアントの依存は `httpx2`（starlette が `httpx` を非推奨化しており、警告解消のため採用）
- DB接続・認証（JWT検証）・lint設定・OpenAPI型生成は本PRのスコープ外

🤖 Generated with [Claude Code](https://claude.com/claude-code)


## Comments

