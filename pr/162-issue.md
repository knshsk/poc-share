title:	[Feature] Viewer: 読取結果エディタの再設計（状態表示・候補値・表組み）
state:	CLOSED

labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#159
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	162
--
### 目的

読取結果タブ（`ResultEditor`）の確認・修正の作業性を上げる。確信度は「色付きの % + バー」の二重表現で大半が赤になり注意喚起として働かず、グリッドは入力欄の羅列で空行も全セルを描画し 6 列表は横スクロールが必須、未保存の下書きと保存済みの修正は同じ見た目で区別できない。

### 変更内容

- `ResultEditor.vue`（現 497 行）を分割する: `ResultEditor`（3 区分・候補値・フォーカス処理）/ `KeyFieldList`（キー項目の行）/ `ResultTable`（表 1 件）。判定は純関数（`utils/fieldStatus.ts`・`utils/table.ts`）に出す。`EditorVm` の型と Mediator のイベントは変えない
- キー項目: 行を「ラベル | 入力 | 状態」の横並び（1 項目 44px）にする。状態は 未保存 / 修正済 / 未読取 / 要確認 n%（80% 未満、warning 色）/ n%（無彩色）。確信度バーと赤は廃止。未保存の入力欄は主色の薄い塗り、キャンバスと連動中の項目は主色 2px の枠
- 候補値（旧: キー項目候補）: チップをやめ、読取専用のテキストボックスにする（選択・コピー可。複数は縦に並べ、0 件は「一致なし」）
- 表（旧: グリッド）: 罫線付きの表にする。セルは枠なしの入力欄、行高 32px、ヘッダー行は背景色 + 太字（編集は可能なまま）、列幅は内容の文字幅の比で配分（6 列表が 528px 幅に横スクロールなしで収まる）、数値のセルは右寄せ、DI にセルが無い座標は入力欄なしのセル
  - 連続する空行は「空行 N 行を表示 / 隠す」に畳む。キャンバスで畳まれた行のセルを選択したら、その塊を自動で展開してセルへフォーカスする
  - 保存済みの修正セルは右上の三角印（読み上げ用のテキストを `aria-describedby` で紐付け）、未保存のセルは薄い塗り
- ドキュメント: `docs/viewer/result-panel.md`、`docs/architecture/viewer.md`、`viewer/README.md`

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- キー項目の 5 状態が仕様どおり表示される（閾値 0.8 の境界を含む）。テスト添付
- 表の畳み / 展開、キャンバス連動時の自動展開、数値の右寄せ、修正済みの印、セルが無い座標が仕様どおり動く。テスト添付
- 注文書サンプルの 6 列表が 528px 幅で横スクロールなしに収まる（実画面で確認）
- 既存の修正・確定・相互ハイライト・アクセシビリティ（`aria-current`・`aria-describedby`・キーボード操作）が維持されている
- 見た目・挙動がモック（`.ignore/mockup/editor.html` の行レイアウト A）と同等
- `npm run test` / `npm run lint` / `npm run build` と pre-commit 通過

### 関連情報

- 親: #159
- #117（3 区分表示と修正 UI）、#118（相互ハイライト）、#127（修正 UI のアクセシビリティ）
- 対象外: 結合セル（`row_span` / `column_span`）の span 描画、候補値の件数上限
- モックアップ: `.ignore/mockup/editor.html`（ローカル管理）

