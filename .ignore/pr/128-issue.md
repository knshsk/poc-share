title:	[Feature] 連鎖修正時に前世代の修正済みキー / セルを判別できるようにする
state:	CLOSED
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
number:	128
--
### 目的

`corrected` 行を base にした再修正（連鎖）では、`run.corrections` に最新の差分だけが保存される（設計 §3.1・ADR-0018）。そのため 2 回目の修正行を表示すると、1 回目に修正したキー / セルの鉛筆マーク（修正済み表示）が消え、「機械が読んだ値」と「人が直した値」の区別が画面上で分からなくなる。

### 変更内容

方針を決めてから実装する。候補:

- A. Viewer が `base_run_id` を辿って先祖の `corrections` をマージし、累積の修正キー集合でマークを付ける（API 変更なし。履歴一覧のサマリには `corrections` が無いため、先祖の詳細を `GET …/idp-runs/{run_id}` で取得しキャッシュする）
- B. API が `corrected` 行に累積の修正キー集合（例: `corrected_keys` / `corrected_cells`）を持たせる（`idp_runs` 列追加または応答時に算出）
- C. 仕様どおり「最新差分のみ」を維持し、`docs/viewer/result-panel.md` に制限として明記する

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 方針（A / B / C）が決まり、B の場合は ADR-0018 に追記
- 連鎖修正後も修正済みキー / セルが判別できる（C の場合は制限をドキュメントに明記）
- テスト添付（A / B の場合）

### 関連情報

- ADR-0018（人手修正の追記保存）、設計 §3.1 / §6.4
- #115（corrections API）、#117（修正済みマーク）

