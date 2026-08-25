title:	[Feature] 認証統合（SPA OIDC/PKCE・API JWT検証・realm定義拡充）
state:	CLOSED
author:	shiro-ino (しろいの)
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
number:	17
--
### 目的

「認証付き骨格が動く」状態を確立する。ViewerのKeycloakログイン（OIDC/PKCE）、APIのJWT検証、realm定義の拡充を通し、以降の機能実装が認証前提で進められる土台を作る。

### 変更内容

- realm定義拡充（`keycloak/realm/factone.json`）
  - publicクライアント `factone-viewer`（SPA・PKCE S256必須）
  - audienceマッパー（アクセストークンへ `factone-api` 付与）
  - 開発用テストユーザー（パスワードは環境変数プレースホルダ。シークレット非含有ルール準拠）
- API: PyJWT導入、Bearerトークン検証の依存性（署名・issuer・audience・exp）、JWKS取得の境界アダプタ化、保護サンプル `GET /api/me`
- API: 認証テスト（正常系＋偽造・期限切れ・issuer不一致・aud不一致等の異常系）
- Viewer: oidc-client-ts導入、Pinia認証ストア（ログイン/コールバック/ログアウト・トークンはメモリ保持）、APIクライアントへのBearer付与、`/api/me` 表示
- ADR: 認証トークン処理ライブラリ選定（PyJWT・oidc-client-ts）
- `.env.example` 拡充、`docs/develop-guide/` へローカル認証動作手順追記

### 完了条件

- [ ] Viewerからログイン→`/api/me` の認証済み応答表示→ログアウトの一連が動作する
- [ ] 未認証・不正トークンでのAPIアクセスが401になる（テストで担保）
- [ ] realm JSONにシークレットが含まれない
- [ ] pre-commit・pytest・Vitestすべて通過
- [ ] ADRが `docs/adr/` に追加されている

### 関連情報

- 仮置き明記: テナント識別・ロール・文書アクセス認可は対象外（未確定事項の解消後に対応）。本Issueは認証（本人確認）のみ

## Comments

