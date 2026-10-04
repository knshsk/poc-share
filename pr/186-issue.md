title:	[Bug] 原本DLの応答前に文書を切り替えると、DL失敗で切替後文書が取得失敗画面になる
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
number:	186
--
### 事象

認証済みの画面で原本DLを押し、応答が返る前に別の文書へ切り替える。その後にDLが失敗すると、切替後の文書を表示していても、画面全体が取得失敗画面（「原本を取得できませんでした」）になる。

**原因:**

`viewer/src/mediator/handlers/public.ts`の`downloadFailed`（DLの失敗を伝えるイベント）は、どの文書のDLだったかを持たない。
受け付けるかどうかは状態（VIEWING）だけで決まるので、切替後の文書が`document`状態になっていれば受け付けてしまう。
ページ画像や実行履歴の応答は、文書IDや読込の世代を見て古い応答を捨てている。DLの失敗だけがその対象から外れている。

#184 の作業中に発見した既存不具合。

### 再現手順

コードの確認による。Mediatorに届くイベントの順で示す。

1. 文書Aを表示した状態で、原本ダウンロードを押す（`downloadOriginal`）
2. ダウンロードの応答が返る前に、文書Bへ切り替える（`openDocument`）
3. 文書Bの読込が終わり、`document`状態になる
4. 文書Aのダウンロードが失敗し、`downloadFailed`が届く

画面で再現するには、ブラウザの開発者ツールで文書Aの`/api/v1/documents/{document_id}/pdf`の応答を遅らせたうえで、失敗させる必要がある。

### 期待する動作

切り替える前の文書のダウンロードが失敗しても、切替後の文書の表示は変わらない。

### 実際の動作

切替後の文書を表示していても、画面全体が取得失敗画面（「原本を取得できませんでした」と再試行ボタン）になる。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。コードの確認による（`viewer/src/mediator/handlers/public.ts`の`downloadOriginal`と`downloadFailed`）。

