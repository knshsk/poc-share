title:	[Bug] 金額の末尾に締めの記号（-など）が付いた数値を正規化できない
state:	OPEN
labels:	bug
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
number:	218
--
### 事象

number型の項目で、金額の末尾に締めの記号を付けた書き方（例: `¥ 74,800 -`）を正規化できず、正規化値がnullになる。

正規化の規則は、NFKCで正規化した後、通貨記号（`¥`）・`円`・カンマ・読点・空白だけを除いて整数にしている。末尾の`-`は除かないので、整数として解釈できない。日本の帳票では、書き足しを防ぐために金額の末尾へ`-`や`也`を付ける書き方が広く使われる。

同じ規則が、サーバ（`api/app/services/idp_extraction.py`の`_normalize`）とViewer（`viewer/src/utils/normalize.ts`）の両方にある。そのため、IDP実行の正規化値と人手修正の再正規化の両方でnullになり、外部システムへ渡す正規化値もnullになる。

### 再現手順

1. Viewerで、成功した実行結果を持つ文書を開く
2. 読取結果タブで、number型の項目（例: 合計金額）に`¥74,800-`と入力する
3. 入力欄の下の表示を見る
4. 修正を確定し、`GET /api/v1/documents/{document_id}/idp-runs/latest`で、その項目の`normalized_value`を見る

### 期待する動作

末尾の締めの記号（`-`、`也`など）を除いてから数値として解釈し、`¥74,800-`を74800にする。先頭の`-`による負数（例: `-12`）は、今までどおり負数として扱う。

### 実際の動作

手順3で、入力欄の下に「数値として解釈できません」と出る。手順4の`normalized_value`はnullになる。

### 対象コンポーネント（複数選択可）

API（`api` 配下）、画面（`viewer` 配下）

### 環境

ローカル。Content Understandingで合成の注文書（`samples/`）を解析したとき、合計金額の原文が`¥ 74,800 -`だった。CUが返す型付きの値は74800だったが、自前の規則で作る正規化値はnullになった。規則はエンジンによらないため、DIの読取値でも同じ原文なら同じ結果になる。

