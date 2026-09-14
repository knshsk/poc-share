title:	[Chore] docs に全体像（L1）・モジュール構成（L2）の文書を新設し、文体を常体に統一する
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
number:	140
--
### 目的

リポジトリ初見の開発者が「全体像 → モジュール構成 → 詳細仕様」の順に段階を踏んで読めるようにする。現状の `docs/` は詳細仕様（テーブル定義・画面部品・要件 ID）のみで、全体像・読み順を示す文書がない。Azure 構成は Bicep を読む以外に把握手段がなく、認証・閲覧 URL の設計は ADR・手順書・テーブル定義に散在している。

### 作業内容

- `docs/README.md` を L0（PoC 要件）→ L1（全体像）→ L2（モジュール構成）→ L3（詳細仕様）の読み順案内に書き換える
- `docs/architecture/overview.md`（L1）を新設する。システム文脈・コンポーネント構成・主要ユースケース（シーケンス図）・データの流れと保存先
- `docs/architecture/api.md` / `viewer.md` / `auth.md` / `view-url.md`（L2）を新設する。各文書は「役割 / 構成 / 主要処理の流れ / 外部依存・設定 / 詳細仕様への導線」の 5 節
- `docs/azure/README.md` を実体化する。トポロジ・Bicep モジュール別の設計・通信と認証経路
- 既存 Markdown（`docs/` 配下と `api/README.md` `viewer/README.md` `keycloak/README.md`）の丁寧語を常体に統一する（内容は変えない）
- `docs/api/README.md` のテナント固有の外部システム名（PoC の題材）を役割名に置き換える。L1/L2 には固有名を書かない
- `CLAUDE.md` の完了条件に「構成・処理フロー・外部依存が変わる変更は L1/L2 を同 PR で更新する」を追記する
- ルート `README.md` に `docs/README.md` へのリンクを 1 行追加する

既存ファイルの移動・ディレクトリ再編は行わない（オーバーレイ型）。

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）, その他

### 完了条件

- [ ] `docs/README.md` が読み順（L0〜L3）を示している
- [ ] `docs/architecture/overview.md` と L2 4 文書、`docs/azure/README.md` が存在し、記載内容がコード（`api/` `viewer/` `bicep/` `keycloak/`）と一致している
- [ ] L1/L2 に「Power Automate」「SAP」の固有名がない
- [ ] 対象 Markdown に丁寧語（ですます調）が残っていない（UI 文言の引用を除く）
- [ ] `docs/` 内の相対リンクがすべて解決する
- [ ] `CLAUDE.md` 完了条件に L1/L2 更新の項目がある
- [ ] pre-commit 通過

### 関連情報

- PR は 2 本に分ける（1: README・overview・azure・文体統一 / 2: L2 4 文書・固有名置換・CLAUDE.md）
- 関連 ADR: 0004, 0005, 0006, 0007, 0008, 0009, 0010, 0011, 0012, 0013, 0015, 0018, 0019
