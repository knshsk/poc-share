title:	[Bug] 画面からの文書登録に失敗すると、APIの英語のエラー文言がそのまま表示される
state:	CLOSED
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
number:	198
--
### 事象

文書選択ダイアログの登録タブで文書の登録に失敗すると、エラーのアラートにAPIの英語の文言が表示される。画面のほかの文言は日本語である。

`viewer/src/mediator/handlers/document.ts`の`register`は、409のときだけ「文書IDは別の内容で登録済みです」と表示する。それ以外の失敗では、APIが返すProblem Detailsの`detail`をそのまま表示する。利用者の入力で起きうる失敗では、次の文言が表示される。

- 文書IDが`/`・`\`を含むとき、または`.`・`..`そのものであるときは、`document_id contains an invalid path segment`
- 文書IDが256文字以上のときは、`Request validation failed`
- 拡張子はPDFだが中身がPDFでないファイルのときは、`Only PDF files are accepted`
- 種別の一覧を取得した後に、その種別の設定が削除されたときは、`Unknown document type for this tenant`

#166 の修正で、文書IDが制御文字を含むときの422が加わる。#189 の修正では、先頭は`%PDF`だがPDFとして開けないファイルも422になる。このときは`Failed to open PDF (corrupted or password-protected)`が表示される。英語の文言が表示される経路は、さらに増える。

#166 の調査（APIとViewerの入力検証の有無）で見つかった。

### 再現手順

1. Viewerで文書選択ダイアログを開き、登録タブを選ぶ
2. 文書IDに`a/b`を入力し、書類種別とPDFファイルを選んで登録する
3. エラーの表示を確認する

### 期待する動作

登録に失敗した理由が、日本語で表示される。文書IDの形式の誤りのように利用者が直せる失敗では、何を直せばよいかが分かる文言を表示する。

### 実際の動作

手順3で、`document_id contains an invalid path segment`と英語で表示される。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。`register`のエラー処理と、APIが返す422の`detail`をコードで確かめた。


