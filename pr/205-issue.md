title:	[Bug] Container Apps上では公開閲覧の接続元IPがingressのIPになり、レート制限を全利用者で共有する
state:	OPEN
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
number:	205
--
### 事象

公開閲覧のAPIは、接続元IP（`request.client.host`）を3つの用途に使う。閲覧イベントの`client_ip`（端末情報）、無効なトークンの失敗記録の`client_ip`（総当りの検知）、IP単位のレート制限のキーである。

uvicornは既定で、接続元が127.0.0.1の要求に限り、`X-Forwarded-For`の値を`request.client.host`にする。ほかの接続元からの要求では`X-Forwarded-For`を使わず、接続元のIPを`request.client.host`にする。本番の起動コマンド（`api/Containerfile`）とContainer Appsの環境変数（`bicep/modules/api-app.bicep`）は、この既定を変えていない。

Container Appsでは、APIに接続するのはingressである。そのため、`client_ip`はすべての要求でingressのIPになる見込みである。Container Appsのingressは、`X-Forwarded-For`の右端に接続元のIPを入れる（Microsoft Learn「Ingress in Azure Container Apps」）。APIはこの値を使っていない。

見込みどおりなら、閲覧イベントの`client_ip`は端末情報として使えず、失敗記録からも総当りの試行元を区別できない。レート制限の枠も、同じingressを通る全利用者で共有する。誰かが60秒の間に無効なトークンで10回アクセスすると、全利用者の公開閲覧が429になる。制限はトークンの検証より先に判定するため、有効な閲覧URLの要求も429になる。

`docs/architecture/view-url.md`は、プロキシの配下では`client_ip`がプロキシのIPになると書いている。レート制限への影響は書いていない。

Azure上では確かめていない。#197 の対応（#204）で、uvicornの`X-Forwarded-For`の扱いを調べたときに見つかった。

### 再現手順

1. Azureの環境で、共有できる文書種別（見積書など）の文書に閲覧URLを発行する
2. 回線の異なる2台の端末（AとB）で閲覧URLを開き、閲覧イベント（`GET /api/v1/documents/{document_id}/events`）の`client_ip`を確かめる
3. 端末Aから、存在しないトークンで`GET /view/{token}/pages`を10回送る。60秒以内に、端末Bで有効な閲覧URLを開く

### 期待する動作

閲覧イベントと失敗記録の`client_ip`には、閲覧者の接続元IPが入る。レート制限は、無効なトークンを送った接続元だけを429にする。手順3では、端末Bは閲覧できる。

### 実際の動作

Azure上では確かめていない。見込みは次のとおり。手順2では、端末AとBの`client_ip`が、どちらも同じingressのIPになる。手順3では端末Bの閲覧も429になり、Viewerは「この閲覧URLは無効です」と表示する。

uvicornの挙動は、ローカルで確かめた。127.0.0.1以外のIPから`X-Forwarded-For`を付けて接続すると、`request.client.host`は接続元のIPになり、`X-Forwarded-For`の値は使われない。127.0.0.1から接続した場合だけ、`X-Forwarded-For`の右端の値が使われる。

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 環境

Azure（Container Apps）。Azure上では確かめていない。uvicornの挙動は、ローカル（uvicorn 0.52.4）で確かめた。

