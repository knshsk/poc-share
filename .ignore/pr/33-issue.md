title:	[Feature] ドキュメント整備（ADR文書化・README現状整合）
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
number:	33
--
### 目的

方針決定済みのアーキテクチャ・方式選定の経緯が設計資料に散在しており、決定の背景・却下案を追跡できるようADRとして記録する。あわせて各ディレクトリのREADMEを現状の実装と整合させる。

### 変更内容

- 決定済み事項のADRを`docs/adr/`へ追加（0003〜0011の9本）
  - 業務キー/文書キー分離、境界アダプタ化、追記型ログ設計
  - Keycloak採用・配置、ネットワーク隔離なし
  - フロント配信Container Apps同居、SPA+OIDC/PKCE、Blob APIプロキシ、閲覧URL不透明トークン
- 各`README.md`（リポジトリルート除く）の現状との齟齬を修正

### 対象コンポーネント

ドキュメント

### 完了条件

- [ ] ADR 0003〜0011が`docs/adr/`に存在し、テンプレート4節構成に従っている
- [ ] 各READMEの記述が現状の実装・構成と一致している

## Comments

