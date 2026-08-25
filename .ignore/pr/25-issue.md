title:	[Feature] 閲覧URL（発行・検証・失効・無認証閲覧）
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
number:	25
--
### 目的

案件型見積PDF向けの閲覧URLを発行し、URLと文書IDを関連付ける。顧客はURLのみで（認証なしで）閲覧でき、発行側は即時失効できる。

### 変更内容

データモデル:

- `view_tokens` — id(UUID・管理用) / token_hash(SHA-256・UNIQUE) / document_id(FK) / issued_at / expires_at(必須・既定90日=設定値) / revoked
- `view_access_failures` — 失敗試行の記録（トークン断片・IP・日時）

API（発行・管理はJWT必須）:

- `POST /api/documents/{document_id}/view-urls` — 発行。顧客共有可の種別（見積のみ）に限定、それ以外は403。トークンは256bit URL-safe乱数、応答で一度だけ平文返却
- `GET /api/documents/{document_id}/view-urls` — 発行済み一覧（平文なし）
- `POST /api/view-urls/{id}/revoke` — 即時失効
- 共有可否は `DocumentType` の属性として宣言（ADR-0002）

顧客閲覧（認証なし・トークンが認可）:

- `GET /view/{token}/pages` / `/view/{token}/pages/{page}/image` / `/view/{token}/pdf`
- 無効トークン（不存在・期限切れ・失効）は一律404+失敗記録。IP単位レート制限（超過429）

Viewer:

- `/view/{token}` パスの無認証閲覧画面（ページ表示+DL）

### 完了条件

- [ ] 発行→閲覧→失効→閲覧不可の一連が動作する
- [ ] 見積以外への発行が403となる
- [ ] 無効トークンが一律404となり、失敗が記録され、レート制限が働く（テストで担保）
- [ ] トークン平文がDB・ログに残らない
- [ ] pre-commit・pytest・Vitest・API契約差分チェックすべて通過

### 関連情報

- ADR-0002（文書種別の共有可否宣言）
- スコープ外: 閲覧・DLイベント記録、URL世代管理、二要素化

## Comments

