title:	[Feature] 閲覧ログ外部連携API（既存イベント一覧の正式契約化）
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
number:	47
--
### 目的

外部連携（Power Automate等）が文書IDを指定して閲覧・DLイベントを取得できるよう、閲覧ログ取得APIを外部連携の正式契約として確定する。

### 変更内容

- 既存 `GET /api/documents/{document_id}/events`（JWT必須）を外部連携の正式契約に昇格
- 応答から `view_token_id` を除去（内部管理値の非公開方針）
- 取得方式はREST pull・文書ID指定・全件返却（`occurred_at` 昇順）。ページネーションなし（連携側が文書ID単位で取得するため件数破綻を考慮不要）
- API契約（OpenAPI）再生成、外部連携向けドキュメント整備
- テスト補強: 応答形状（`view_token_id` 非含有）・0件・文書不存在404・未認証401

### 完了条件

- [ ] M2Mクライアントのトークンで文書IDを指定し全イベントを取得できる
- [ ] 応答に `view_token_id` が含まれない
- [ ] pre-commit・pytest・API契約差分チェックすべて通過

### 関連情報

- Issue #27（閲覧・DLイベント記録。検証用一覧APIとして先行実装済）

## Comments

