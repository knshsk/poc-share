title:	[Feature] Viewer UI: 作業パネル刷新
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
number:	77
--
### 目的

Viewer UIプロダクトレベル化（#74）の作業パネル（読取結果確認・照合支援）実装。

### 変更内容

読取結果/実行履歴/閲覧URL(quoteのみ)のタブ化（WorkPanel）。確信度3段階の視覚化（90%/80%閾値）、失効確認ダイアログ、発行URL再表示不可の明示。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- タブ切替が文書種別に応じて構成される
- 確信度が3段階で色分け表示される
- 失効に確認ダイアログが入る
- npm run test / lint / build 通過

### 関連情報

- 親: #74
