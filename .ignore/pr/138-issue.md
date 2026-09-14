title:	[Feature] 閲覧用URLの文書単位一括失効API
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
number:	138
--
### 目的

見積改訂時に旧版の閲覧用URLをまとめて失効させたい（operation-flow K②-5: Power Automate が旧版の閲覧用URL失効APIを要求）。現状は `POST /api/v1/view-urls/{view_url_id}/revoke` で1件ずつ失効する必要があり、連携側が一覧取得→個別失効のループを組む負担がある。

### 変更内容

- `POST /api/v1/documents/{document_id}/view-urls/revoke-all` を追加
- 対象文書の未失効トークンを一括で `revoked=true` にする
- 応答: 200 `{"status": "revoked", "revoked_count": N}`（対象0件でも200・冪等）
- 文書不存在・他テナントは 404 `document-not-found`
- 認証必須。新規エラーコードなし
- OpenAPI / TypeScript 型を再生成

### 対象コンポーネント（複数選択可）

API（`api` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] エンドポイント追加
- [ ] テスト（一括失効後の閲覧404・件数・既失効除外・未知文書404）
- [ ] openapi.json / schema.d.ts 再生成
- [ ] pre-commit 通過

### 関連情報

- ADR-0011（閲覧URL不透明トークン・即時失効）
- docs/business/operation-flow.md K②-5
