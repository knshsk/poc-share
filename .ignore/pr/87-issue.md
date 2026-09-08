title:	[Bug] ViewerのURLパラメータ document_id がログイン後に失われる
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
number:	87
--
### 事象

ViewerのURLパラメータ `document_id` が機能しない。パラメータ付きでアクセスすると、ログイン後に書類ID入力欄（empty state）が表示され、指定した文書が開かれない。

### 再現手順

1. 未認証状態で `http://localhost:5173/?document_id=<文書ID>` にアクセスする
2. ログイン画面で「ログイン」を押し、Keycloakで認証する
3. Viewerに戻る

### 期待する動作

ログイン後、URLパラメータで指定した文書が表示される。

### 実際の動作

URLが `/` に置き換わり、書類ID入力欄が表示される。

### 原因

- `viewer/src/App.vue` のコールバック処理で `history.replaceState({}, '', '/')` を行い、ログイン前のクエリ文字列が破棄される
- アクセストークンはメモリ保持のみのため、ページ読込時は常に未認証となり必ずログイン画面を経由する（設計意図どおり）。そのためログイン前URLの保持が必須

### 対応方針

- `signinRedirect` の `state` にログイン前の `pathname + search` を保存し、`signinCallback` 後にその URL へ `replaceState` する
- 復元先は相対パス（`/` 始まり・`//` 不可）のみ許可する
- セッション永続化・サイレント認証はスコープ外

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル

