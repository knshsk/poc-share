title:	[Bug] 原本DLボタンを連打すると取得要求が重なり、閲覧URLでは閲覧ログにDLが重複して記録される
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
number:	193
--
### 事象

原本DLのボタン（閲覧URLの画面では「ダウンロード」）は、応答を待つ間も押せる。応答が返る前に押した回数だけ原本PDFの取得要求が重なって出る。

**原因**

`viewer/src/mediator/handlers/public.ts`の`downloadOriginal`は、応答待ちの要求があっても新しい要求を出す。`viewer/src/components/DocumentCanvas.vue`のボタンにも、応答待ちの表示や無効化が無い。

閲覧URL経由の`GET /view/{token}/pdf`は、要求のたびに閲覧ログ（`view_events`）へ`download`を記録する。冪等キーはマイクロ秒単位の発生日時を含むため、連続した要求は別々の行になる。閲覧URLのレート制限は無効なトークンでの失敗だけを数えるので、有効な閲覧URLでの連続した要求は止まらない。

認証済みの`GET /api/v1/documents/{document_id}/pdf`も、要求のたびにBlobから原本を読む。

#186 の修正（#192）の動作確認中に発見した既存不具合。

### 再現手順

1. 認証済みの画面か閲覧URLの画面で、文書を表示する
2. ブラウザの開発者ツールでNetworkタブを開く
3. 「原本ダウンロード」のボタン（閲覧URLの画面では「ダウンロード」）を、素早く何度も押す

### 期待する動作

応答を待つ間は、原本の取得要求を重ねて出さない。

### 実際の動作

応答を待つ間に押した回数だけ、`.../pdf`への要求が出る。応答が返るたびに、ファイルを保存する処理が動く。閲覧URLの画面では、閲覧ログに`download`が要求の回数だけ記録される。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。コードの確認による（`viewer/src/mediator/handlers/public.ts`の`downloadOriginal`、`viewer/src/components/DocumentCanvas.vue`のダウンロードボタン、`api/app/routers/view_urls.py`の`public_pdf`）。

