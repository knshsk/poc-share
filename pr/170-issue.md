title:	[Feature] キャンバスの「高さに合わせる」を削除し、フィットを「ページ全体」「幅」の 2 つにする
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
number:	170
--
### 目的

右ペインの幅を広げてキャンバスを狭めると、「高さに合わせる」のときだけページの高さが保たれて横スクロールバーが出る。「ページ全体を表示」「幅に合わせる」は表示領域に合わせて縮むため、操作感がそろわず違和感がある。

主要な PDF ビューアでも、フィットは表示領域の変化に追従する（pdf.js と Chrome はリサイズ時にフィットを再適用し、Acrobat の「幅に合わせる」も表示領域に対する相対指定）。一方で、pdf.js と Chrome のボタンは「ページ全体」「幅」だけで、高さ合わせは持たない。表示領域が十分広いときは「高さに合わせる」と「ページ全体を表示」は同じ倍率になり、差が出るのは幅が制約になったときの横スクロールだけ。一般的なビューアと同じ構成にそろえ、違和感の出る状態をなくす。

### 変更内容

- キャンバスのツールバーから「高さに合わせる」を削除する。フィットは「幅に合わせる」「ページ全体を表示」の 2 つ（表示領域の変化に追従し、± の操作で手動になる）。初期表示は「ページ全体」のまま
- `viewer/src/utils/zoom.ts`: `ZoomMode` から `height` を除き、`fitZoom` を幅合わせ・ページ全体の 2 種にする
- `viewer/src/components/DocumentCanvas.vue`: 高さ合わせのボタンを削除する。CSS の `ponytail:` 注記（横スクロールバーの高さを考慮しない）の前提を、「手動の倍率で横にはみ出しているときにフィットを押した場合」に改める
- テスト: 高さ合わせのケースを削除し、倍率の下限 1・25〜200% に制限しないこと・± のスナップはページ全体で確認する
- ドキュメント: `docs/viewer/display.md`（モード・フィットの定義・ツールバー）、`viewer/README.md`

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- ツールバーのフィットが「幅に合わせる」「ページ全体を表示」の 2 つになっている（公開閲覧も同じ）
- 右ペインの幅を変えたとき、「ページ全体」「幅」は表示領域に追従し、手動の倍率は保たれる（倍率ラベルと表示サイズは連動したまま）
- テストと pre-commit が通り、`docs/viewer/display.md` が実装と一致している

### 関連情報

- フィット 3 種は #116 で導入した。本件は #168（PR #169、倍率をページ実寸基準にした修正）の確認中に出た指摘
- 主要ビューアの挙動の出典: pdf.js `web/app.js` の `onResize`、Chromium `chrome/browser/resources/pdf/viewport.ts` の `resize_`、Acrobat Users フォーラム「"fit width" shows different zoom level in Reader vs Pro」

