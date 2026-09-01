title:	[Bug] 文書種別のテナント別設定値化（#81）のviewer側対応漏れ
state:	CLOSED
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
number:	85
--
### 事象

#81 で文書種別をテナント別設定値化（`document_type_configs`）したが、viewer側に設定値を参照せずハードコードされたままの箇所が残っている。

1. `viewer/src/components/AppHeader.vue` — 種別チップの表示名が `typeLabels` 定数にハードコード。設定の `display_name` と齟齬が生じる
2. `viewer/src/components/WorkPanel.vue` — 閲覧URLタブの表示制御が `documentType === 'quote'` のハードコード判定。設定の `shareable` を参照していない

### 再現手順

1. `PUT /api/config/document-types/quote` で `display_name` を「見積」以外に変更する
2. viewerで見積文書を表示する
3. ヘッダーの種別チップを確認する

### 期待する動作

- 種別チップに設定の `display_name` が表示される
- 閲覧URLタブの表示可否が設定の `shareable` に従う

### 実際の動作

- 種別チップはハードコードされた「見積」「注文書（案件）」「注文書（ルート）」を表示。未知の種別コードでは表示が空になる
- 閲覧URLタブは `quote` 種別のみ表示され、`shareable` 設定を反映しない

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル / Azure 共通（実装起因）
