title:	[Bug] アプリバーの文書タイトルの表示不整合
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
number:	187
--
### 事象

アプリバーの文書タイトルについて、各種処理中（文書の読込中・IDP実行中・修正の確定中）でもボタンの外観が変わらない。処理中にボタンを押下しても何も起きないが、反応しない理由も表示されないため、ユーザからは操作できないことがわからない。

これらの状態では、Mediatorが`openDocumentPicker`を受け付けない（`viewer/src/mediator/handlers/document.ts`）。ボタンの側は状態を見ていないので、見た目もスクリーンリーダーに伝わる状態も変わらない。

#184 の作業中に発見した既存動作。

### 再現手順

1. 文書を表示する
2. ブラウザの開発者ツールで通信速度を落とし、文書選択ダイアログの「文書IDで参照」から別の文書を参照する
3. 読込中（スピナーの表示中）に、アプリバーの文書タイトルを押す

IDP実行中と確定中も同じ動きになる。IDP実行は実際のDocument Intelligenceを呼ぶため、再現には読込中を使う。

### 期待する動作

ボタンの見た目と実際の反応が一致する。押しても反応しない間は、そのことが見た目とスクリーンリーダーで分かる。

### 実際の動作

ボタンは押せる見た目のままで、押しても何も起きない。押せない状態であることは、スクリーンリーダーにも伝わらない。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。コードの確認による（`viewer/src/mediator/handlers/document.ts`の受付状態、`viewer/src/components/AppHeader.vue`）。

