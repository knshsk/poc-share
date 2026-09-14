title:	[Feature] Viewer: 読取結果の修正 UI と相互ハイライトのアクセシビリティ改善
state:	CLOSED
labels:	accessibility, feature
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
number:	127
--
### 目的

#117 / #118 で追加した読取結果の修正 UI と相互ハイライトは、マウス操作と色による状態表示を前提に実装した。PoC としては許容したが、スクリーンリーダー・キーボード利用者向けの基礎的なアクセシビリティが不足している。

### 変更内容

- キャンバスの読取矩形（`PageSvg.vue` の `<rect>`）: `<title>`（項目ラベル / 表 n の r 行 c 列）でツールチップとアクセシブル名を付け、`tabindex="0"` + `role="button"` + Enter / Space でクリックと同じ選択を行えるようにする
- 正規化ヒント（`ResultEditor.vue`）: 入力欄に `aria-describedby` でヒント要素を紐付ける（`hide-details` のため messages スロットは使えない）
- 確認ダイアログ「未保存の変更を破棄しますか？」（`WorkPanel.vue`）: `aria-labelledby` でタイトルを紐付ける
- 修正済みマーク（鉛筆アイコン）: `role="img"` + `aria-label="修正"`（ツールチップはマウス限定のため）
- 選択中入力欄（`target--active`）: 色以外の手掛かり（例: 左端のアイコンまたは `aria-current`）を追加する
- `scrollIntoView({ behavior: 'smooth' })` は `prefers-reduced-motion: reduce` のとき `auto` にする
- 修正入力欄に `maxlength="2000"`（サーバ側 `CorrectionText` の上限と一致）

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- 上記の各要素に対する Vitest（属性・キーボード操作でのストア更新）が追加されている
- `npm run test` / `npm run lint` / `npm run build` 通過
- `docs/viewer/result-panel.md` / `docs/viewer/display.md` にキーボード操作を追記

### 関連情報

- #117（ResultEditor / WorkPanel）、#118（PageSvg / highlight）
- 各 PR の最終レビューで先送りした a11y 指摘のまとめ

