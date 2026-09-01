title:	[Feature] Viewer UI: 2ペインレイアウトとドキュメントキャンバス
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
number:	76
--
### 目的

Viewer UIプロダクトレベル化（#74）の主軸シナリオ（原本照合）向けレイアウト実装。

### 変更内容

DocumentViewを左（ページ表示+ナビ/ズーム）右（作業パネル）の2ペイン化。文書コンテキストのヘッダ表示、empty state / エラー / スケルトンの状態設計。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- ページ送り・ズーム・幅合わせが動作
- 幅960px未満で縦積みフォールバック
- 文書ID未指定時にempty state表示
- npm run test / lint / build 通過

### 関連情報

- 親: #74
