title:	[Feature] Viewerで明細を表示・修正できるようにする
state:	OPEN
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
number:	216
--
### 目的

CUで取った明細を、原本と並べて確認・修正できるようにする。

### 変更内容

- 読取結果タブに、array型の項目（明細）を表示する
- 明細の値を修正し、確定できるようにする
- 明細の値と、キャンバス上の根拠の矩形を相互にハイライトする

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）、ドキュメント（`docs` 配下）

### 完了条件

- 明細の各値を表示・修正・確定できる
- 明細の値と、キャンバス上の根拠の矩形を相互にハイライトできる

### 関連情報

- 親: #213
- 前提: #215

