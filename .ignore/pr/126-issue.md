title:	[Chore] docs/viewer/display.md の閲覧イベント記録に関する古い記述を削除する
state:	CLOSED
labels:	chore, documentation
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
number:	126
--
### 目的

`docs/viewer/display.md` の「実装ノート」に、閲覧・DLイベント記録を「後続で追加する」と書かれた記述が残っている。閲覧イベント記録（ADR-0013 / ADR-0015）は実装済みで、同じ文書の 10 行目でページ単位の `page_view` 記録を説明しており矛盾している。

### 作業内容

- `docs/viewer/display.md` の実装ノートから「閲覧・DLイベント記録は本方式の配信経路（APIプロキシ）上に後続で追加する」の 1 行を削除する
- 同節の残り（オンデマンド変換）はそのまま

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- `docs/viewer/display.md` に閲覧・DLイベント記録を未実装とする記述が無い
- pre-commit 通過

### 関連情報

- ADR-0013（閲覧イベントの差分フィード）、ADR-0015（ページ閲覧イベント）
- #118 の最終レビューで検出（#112 の後続整理）

