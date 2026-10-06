title:	[Bug] ログインから5分を過ぎると、ViewerのAPI呼出がすべて401になる
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
number:	209
--
### 事象

Viewerは、ログインの完了時（OIDCのコールバック）に受け取ったアクセストークンを、APIクライアントに1回だけ設定する（`viewer/src/mediator/handlers/auth.ts`の`callbackDone`）。トークンを更新して設定し直す処理は無い。

アクセストークンの寿命は300秒である。realm定義（`keycloak/realm/factone.json`）で寿命を指定していないため、Keycloakの既定値になる。APIはJWTの`exp`を検証するので、ログインから300秒を過ぎた後の要求は、すべて401 `invalid-token`になる。

そのため、ログインから5分を過ぎると、認証済みの画面でAPIを呼ぶ操作がすべて失敗する。例えば、ページ画像と実行履歴の取得、IDP実行、修正の確定、閲覧URLの発行と失効、原本のダウンロードが失敗する。Viewerを再読込してログインし直すまで回復しない。未保存の修正は確定できず、再読込すると失われる。

ADR-0009は、アクセストークンを短寿命にし、refresh token rotationで更新すると決めている。今の実装は、この決定のうちトークンの更新を満たしていない。

### 再現手順

1. Viewerにログインし、文書を開く
2. ログインから5分以上待つ
3. 「原本ダウンロード」を押す

### 期待する動作

Keycloakのログインセッションが続いている間は、アクセストークンが期限切れになる前に更新され、APIを呼ぶ操作を続けられる。

### 実際の動作

ログインから300秒を過ぎると、APIは次の応答を返す。

```
HTTP 401
{"type":"/errors/invalid-token","title":"Invalid token","status":401,"detail":"Invalid token"}
```

画面には、操作ごとの失敗の表示が出る。手順3では、スナックバーに「ダウンロードできませんでした」が出る。ページ画像の取得では、紙面に「ページを表示できませんでした」が出る。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル。APIの401は、M2Mクライアント（`factone-integration`）のトークンで実測した。発行の直後は200、305秒後は401を返した。画面の挙動は、コードの確認による。Azure環境のKeycloakも、同じrealm定義を取り込んだイメージで動く（`keycloak/Containerfile`）。

