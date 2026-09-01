title:	[Feature] Viewer UI: テーマ・シェル刷新
state:	CLOSED
author:	shiro-ino (しろいの)
labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#74
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	75
--
### 目的

Viewer UIプロダクトレベル化（#74）のビジュアル基盤整備。

### 変更内容

Vuetifyカスタムテーマ（Fluent Azureパレット・カラートークン一元管理）、IBM Plexフォント、Phosphor Icons導入。App.vueのシェル分離（AppHeader・ログイン画面）とデバッグ要素除去。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- テーマトークンがvuetify.tsで一元管理されている
- アプリバー・ログイン画面が刷新されている
- /api/me表示等のデバッグ要素が除去されている
- npm run test / lint / build 通過

### 関連情報

- 親: #74
