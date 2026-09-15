title:	[Feature] JWT の tenant_id クレームでテナントを解決しデータを分離する
state:	CLOSED
labels:	feature
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
number:	147
--
### 目的

マルチテナント SaaS として複数テナントを収容できるようにする。現状はテナントが設定値 `FACTONE_TENANT_ID=default` で固定されており、JWT からテナントを解決する仕組みがない。本稼働時に統合する社内 SaaS の Keycloak が単一 realm + テナント ID クレーム方式のため、同じ方式に揃える。

### 変更内容

- `core/auth.py` で JWT の `tenant_id` クレームを取り、欠落時は 403 `tenant-unresolved`
- 全ルーターの `settings.tenant_id` を `user.tenant_id` に置き換え、`FACTONE_TENANT_ID` を廃止
- `documents` を内部 UUID 主キーにし `(tenant_id, document_id)` を UNIQUE に。子テーブルの外部キーを UUID に張り替え
- 閲覧 URL の単体失効にテナント検査を追加
- Keycloak realm 定義にユーザー属性 `tenant_id` とマッパーを追加
- テナント作成・ユーザー追加・無効化のスクリプト `scripts/tenant.sh` と手順書
- ADR 2 本と設計文書の更新

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 認証（`keycloak` 配下）, スクリプト（`scripts` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- クレーム欠落で 403、他テナントの文書・トークン・設定・実行ログに触れると 404 になるテストが通る
- 同一 `document_id` を 2 テナントで登録できる
- マイグレーションの upgrade / downgrade が通り、既存行の `document_pk` が埋まる
- ローカルスタックで SPA ログインと M2M トークンの両方に `tenant_id` クレームが入る
- `scripts/tenant.sh create/add-user/disable/enable` が動く
- 設計文書・ADR・OpenAPI が更新されている

### 関連情報

ADR-0014（テナント別文書種別設定）
