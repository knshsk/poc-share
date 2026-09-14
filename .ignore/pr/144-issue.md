title:	[Chore] コードコメントと実装のずれを修正する（api-app.bicep の ID 種別、FailureRateLimiter の窓方式）
state:	CLOSED
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
number:	144
--
### 目的

docs 整備（#140）の実装照合で見つかった、コードコメントが実装と食い違っている箇所を直す。動作変更はない。

### 作業内容

- `bicep/modules/api-app.bicep` 冒頭コメント: 「システム割当マネージドID」とあるが、実装は `identity.type: 'UserAssigned'`（`modules/identities.bicep` のユーザー割当 ID）。コメントを「ユーザー割当」に直す
- `api/app/routers/view_urls.py` の `FailureRateLimiter` docstring と `api/app/core/config.py` の `view_url_max_failures` 付近のコメント: 「固定窓」とあるが、実装は deque を毎回 prune するスライディング窓（直近 N 秒の失敗回数）。docstring・コメントを「スライディング窓」に直す。`docs/data-model/view-tokens.md` の「固定窓レート制限」も同じく修正する

### 対象コンポーネント（複数選択可）

API（`api` 配下）, IaC（`bicep` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] 上記 2 箇所のコメント / docstring が実装と一致している
- [ ] `docs/data-model/view-tokens.md` の窓方式の記述が実装と一致している
- [ ] pre-commit 通過

### 関連情報

- #140、#141、#142（`docs/architecture/view-url.md` と `docs/azure/README.md` は実装どおり「スライディング窓」「ユーザー割当」で記載済み）
