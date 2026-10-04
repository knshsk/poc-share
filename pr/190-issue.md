title:	[Bug] 書類種別一覧の取得エラーが画面に反映されない
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
number:	190
--
### 事象

書類種別の一覧の取得（`GET /api/v1/config/document-types`）に失敗すると、文書選択ダイアログの「文書を登録」タブで、書類種別の選択肢が空になる。取得に失敗したことは、画面に示されない。書類種別を選べないので登録ボタンは押せないままだが、利用者には理由が分からない。

一覧が空なら、ダイアログを開き直したときに取り直す（`viewer/src/mediator/handlers/document.ts`の`openDocumentPicker`）。ただし、開き直せば取り直すことも、利用者には分からない。

### 再現手順

1. ブラウザの開発者ツールで、`/api/v1/config/document-types`への要求を遮断する
2. URLに`document_id`を付けずにログインする（ダイアログが自動で開く）
3. 「文書を登録」タブの書類種別の選択肢を開く

### 期待する動作

書類種別の一覧を取得できなかったことが、利用者に分かる。

### 実際の動作

書類種別の選択肢が1つも出ない。エラーの表示は無く、登録ボタンは無効のままになる。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。コードの確認による（`viewer/src/mediator/handlers/document.ts`の`openDocumentPicker`、`viewer/src/components/DocumentRegisterForm.vue`）。

