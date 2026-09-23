title:	[Feature] Viewer: 右ペイン幅の可変化と「表示中」表示・一覧取得失敗フラグ
state:	CLOSED

labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#159
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	161
--
### 目的

右ペインの幅が 528px 固定で、列の多い表や長い値を扱うときに広げられない。上部の選択中結果チップ（`修正 · 2026/9/18 2:24:03 · testuser`）は意味が伝わりにくく、過去の結果を表示していても気付けない。あわせて UI 是正 第1弾（#157）の持ち越しを解消する。

### 変更内容

- `DocumentView`: キャンバスと右ペインの境界（幅 6px、`role="separator"`）を追加する
  - ドラッグ（`setPointerCapture`。window へのリスナーは足さない）/ ←→ キー（16px）で幅を変更、ダブルクリックで既定 528px。最小 400px、最大はキャンバスが 320px 残る幅（上限は CSS の `min()` で効かせる）
  - 幅は保存しない（リロードで既定に戻る）。幅 960px 未満の縦積みでは境界を出さない
  - キャンバスのフィットは既存の `ResizeObserver` が追従する
- `WorkPanel`: 選択中結果のチップを「表示中 {区分}（{修正者}）{日時}」+「最新」/「過去の結果」（warning 色）に置き換える。クリックで実行履歴タブへ（既存の `tabChanged`）。実行履歴の先頭行にも「最新」を付ける
- Mediator: `Model.runsLoadFailed` を追加し（`runsFailed` で true、再取得・成功・文書切替で false）、`WorkPanelVm` に渡す。`WorkPanel` の `noRuns` の `!vm.error` をこれに置き換える（実行 0 件の文書で IDP 実行が失敗した直後も、塗りと案内が残る）
- 文言: キャンバスのダウンロードボタン「原本DL」→「原本ダウンロード」（公開閲覧の「ダウンロード」は変えない）
- `ViewUrlPanel`: 期限の行を 12px にして発行日時（14px）と階層を付ける
- ドキュメント: `docs/viewer/display.md`、`docs/viewer/result-panel.md`、`docs/architecture/viewer.md`

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 境界のドラッグ・キー操作・ダブルクリックで右ペイン幅が変わり、キャンバスのフィットが追従する。最小 / 最大で止まる。テスト添付
- 最新でない実行を選ぶと「過去の結果」が表示され、クリックで実行履歴タブへ移る。テスト添付
- 一覧の取得失敗時は IDP実行が塗りにならず案内も出ない。IDP 実行失敗だけなら塗りと案内が残る。テスト添付
- `npm run test` / `npm run lint` / `npm run build` と pre-commit 通過

### 関連情報

- 親: #159
- #157（第1弾。`noRuns` の制約と閲覧URL 一覧の階層は第1弾の持ち越し）
