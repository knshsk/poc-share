title:	[Chore] Viewer負債: ResultEditor.cellAt の線形探索と横スクロールバー高さの未考慮
state:	OPEN
labels:	chore
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
number:	130
--
### 目的

#116 / #118 で意図的に残した実装上の上限（`ponytail:` コメント）を追跡する。現状のテーブル規模・レイアウトでは問題にならないが、条件を満たしたら対応する。

### 作業内容

- `viewer/src/components/ResultEditor.vue` `cellAt`: 座標ごとの線形探索（1 描画あたり O(cells²)）。任意の入力 1 打鍵で全セルが再評価される。表が数百セル規模になったら `cellKey → cell` の `Map` を `computed` で持つ（数行）
- `viewer/src/components/DocumentCanvas.vue` `.canvas-scroller`: `scrollbar-gutter: stable` は縦スクロールバーのみ考慮。「高さに合わせる」で倍率が 100% を超える場合（横長ページ・縦長ペイン）の横スクロールバー高さは未考慮で、数 px 縦にはみ出す。必要になれば `offsetHeight - clientHeight` で補正
- `docs/viewer/display.md` / `result-panel.md` の該当箇所に上限を明記するか、対応後にコメントを外す

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- 上記 2 点について対応する / しない（上限を文書化）を決めて実施
- 対応した場合はテスト添付、`ponytail:` コメントを削除

### 関連情報

- #116（フィットズーム）、#118（相互ハイライト）

