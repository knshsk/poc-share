title:	[Feature] Viewer UIのプロダクトレベル化（親Issue）
state:	CLOSED
author:	shiro-ino (しろいの)
labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	
sub-issues:	shiro-ino/factone-idp-viewer#75, shiro-ino/factone-idp-viewer#76, shiro-ino/factone-idp-viewer#77
sub-issues-completed:	3/3
blocked-by:	
blocking:	
number:	74
--
### 目的

現在のViewer UIはKeycloak認証連携とAPI動作確認を行う程度の簡素なものであり、プロダクトレベルのUI/UXへ改良する。

### 変更内容

原本照合（注文書）を主軸とした2ペインレイアウトへの刷新。設計は docs/superpowers/specs/2026-09-01-viewer-ui-enrichment-design.md（ローカル管理）に基づく。

Sub-issueで分割実施:
1. テーマ・シェル刷新（Fluent Azureパレット・Phosphor Icons・アプリバー）
2. 2ペインレイアウト + ドキュメントキャンバス（ページナビ・ズーム）
3. 作業パネル刷新（タブ化・確信度バー・失効確認ダイアログ）

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- 全sub-issueのPRがマージされている

### 関連情報

- ADR-0002（文書種別の顧客共有可否）
