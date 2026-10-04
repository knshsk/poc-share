title:	[Bug] 原本DLの失敗で取得失敗画面になり、その後の再試行・参照・登録で未保存の修正が確認なしに消える
state:	CLOSED
author:	shiro-ino (しろいの)
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
number:	191
--
### 事象

認証済みの画面で、読取結果に未保存の修正があるまま原本ダウンロードが失敗すると、画面全体が取得失敗画面になる。画面には「原本を取得できませんでした」と再試行ボタンが出る。ここで再試行を押すと、未保存の修正が破棄の確認なしに消える。文書選択ダイアログから文書を参照・登録したときも、同じく消える。

**原因**

`viewer/src/mediator/handlers/public.ts`の`downloadFailed`は、認証済みの画面でダウンロードが失敗したとき、状態を`documentError`にする。`documentError`は文書の取得失敗と同じ全画面エラーである。文書は表示できているのに、ダウンロードが失敗しただけで、作業中の画面が取得失敗画面に切り替わってしまう。

未保存の修正はモデルに残る。しかし`documentError`の間に受け付ける`retryLoad`・`openDocument`・`register`は、破棄の確認（`guardDirty`）を挟まない。破棄の確認を`document`状態のときだけ行うためである。どの操作も、`clearDocument`で修正を消す。

IDP実行中や確定中にダウンロードが失敗した場合も、同じく取得失敗画面になる。実行・確定の結果は、再試行するまで画面に出ない。

#186 の作業中に発見した既存不具合。

### 再現手順

1. 文書を表示し、読取結果タブでキー項目を修正する。確定はしない
2. ブラウザの開発者ツールで、`/api/v1/documents/{document_id}/pdf`へのリクエストをブロックする
3. 原本ダウンロードを押す
4. 取得失敗画面で再試行を押す。または、文書選択ダイアログから文書を参照する

### 期待する動作

原本ダウンロードが失敗しても、未保存の修正が確認なしに失われない。

### 実際の動作

手順3で、画面全体が取得失敗画面になる。手順4では文書が読み込み直され、未保存の修正が確認なしに消える。

取得失敗画面の間も修正はモデルに残っていて、ページを離れるときの警告（`beforeunload`）は出る。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。コードを読み、Mediatorを単体テストの形で動かして確かめた。該当するコードは、`viewer/src/mediator/handlers/public.ts`の`downloadFailed`と、`viewer/src/mediator/handlers/document.ts`の`retryLoad`・`openDocument`・`register`。

