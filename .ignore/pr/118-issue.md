title:	[Feature] Viewer: キャンバス⇄右ペインの相互ハイライト
state:	CLOSED
labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#112
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	118
--
### 目的

原本照合で「どの値を書類のどこから読んだか」を即座に突き合わせられるように、キャンバス上の読取領域と右ペインの入力欄を相互に連動させる。

### 変更内容

- `PageSvg`: `rects` を `{id, kind: 'field'|'cell', x, y, width, height}` に拡張し、`activeId` prop と `rect-click(id)` emit を追加。polygon → 外接矩形（インチ、viewBox と同一単位で無変換）。`field` は破線、`cell` は実線。選択中は primary 32% 塗り + `primary-darken-1` の太枠、ホバーで塗りを強める（semantic color は使わない）
- 新規 Pinia ストア `highlight`: `activeId`（`field:{key}` / `cell:{table}:{row}:{col}`）、ペイン→キャンバスの「矩形を表示範囲へ」要求、キャンバス→ペインの「入力欄をフォーカス」要求（連番付き）
- `DocumentCanvas`: 選択中の成功実行から矩形を生成（セル = polygon あり全件、キー項目 = `field_regions` の領域あり・`visible=true`・`raw_fields` 非 null）。`page_number` ごとに該当ページの `PageSvg` へ渡し、セルを先・キー項目を後に描画。要求に応じて矩形要素を `scrollIntoView({block: 'center'})`
- `ResultEditor` / `WorkPanel`: 入力欄フォーカスで `activeId` を設定。矩形クリックで読取結果タブへ切り替え、該当入力欄を `scrollIntoView` + `focus()`。選択中入力欄は primary 10% 背景 + 1px 枠。選択はフォーカスが外れても維持し、履歴選択・IDP実行で解除
- ドキュメント: `docs/viewer/display.md`（矩形オーバーレイへのデータ供給）、`docs/viewer/result-panel.md`（相互ハイライト）

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 入力欄フォーカスで対応矩形が強調され、矩形クリックで対応入力欄がフォーカスされる（テスト添付: PageSvg の ID 付き矩形・active クラス・クリック emit、highlight ストア、DocumentCanvas の矩形生成）
- 座標なし項目・失敗実行・未選択では矩形が描画されない
- 実装はモックアップ（`.ignore/mockup/`）と同等の見た目・挙動になっている
- `npm run test` / `npm run lint` / `npm run build` 通過
- `/ponytail:ponytail-review` でレビュー済み

### 関連情報

- 親: #112
- 依存: #113（DI 生応答の保存・`field_regions`）、#117（読取結果の 3 区分表示と修正・確定 UI）
- `docs/viewer/display.md`（座標系整合: DI polygon と SVG viewBox はともにインチ）

