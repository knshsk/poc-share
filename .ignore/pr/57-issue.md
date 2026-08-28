title:	[Chore] docs/requirements新設とIDP/Viewer機能要件資料の格納
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
number:	57
--
### 目的

IDP/Viewerに求められる機能要件が `docs/business/`（operation-flow.md のレーン定義・各工程、out-of-scope.md）に分散記載されており、コンポーネント単位で参照できる機能要件資料がないため。

### 作業内容

- `docs/requirements/` ディレクトリを新設する
- `README.md`（目的・対象・記述方針・一覧、IDP/Viewer共通事項）を作成する
- `viewer.md`（Viewer機能要件。要件ID VW-xx、出所工程・関連対象外事項の紐付け）を作成する
- `idp.md`（IDP機能要件。要件ID IDP-xx、出所工程・関連対象外事項の紐付け）を作成する
- `docs/README.md` のディレクトリ一覧に `requirements/` を追記する

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- [ ] `docs/requirements/` に README.md・viewer.md・idp.md が格納されている
- [ ] 各要件が `docs/business/` の出所（工程・レーン定義・OUT-xx）へ参照付きで整理されている
- [ ] `docs/README.md` に `requirements/` の行が追加されている

### 関連情報

- #53（docs/business新設）
- ADR-0011（閲覧用URLの不透明トークン）・ADR-0012（社内閲覧イベント非履歴化）

## Comments

