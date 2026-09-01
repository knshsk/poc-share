title:	[Chore] api/app配下のレイヤー型ディレクトリ再構成とapi/README.md作成
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
number:	68
--
### 目的

api/app 配下のモジュールがフラット構造のため、どのモジュールを編集すべきか・どこに新規作成すべきかの指針が不明瞭。FastAPI推奨のレイヤー型構造へ再編し、他メンバーの開発時の判断基準を整備する。

### 作業内容

- git mv によるファイル移動と import 修正のみ（ロジック変更なし）
  - core/: config, auth, db, static_serving
  - routers/: documents, idp_runs, view_urls
  - services/: blob, di, idp_extraction, pdf_render
  - main.py / models.py は app/ 直下に維持
- api/README.md 新規作成（ディレクトリ構成・各ディレクトリの責務・新規モジュール配置指針）

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 完了条件

- 既存テスト全通過（uv run pytest）
- pre-commit 通過
- api/README.md がディレクトリ責務と配置指針を記載

### 関連情報

なし
