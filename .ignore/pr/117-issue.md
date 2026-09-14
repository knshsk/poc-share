title:	[Feature] Viewer: 読取結果の 3 区分表示と修正・確定 UI
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
number:	117
--
### 目的

右ペインを読取結果の確認・修正の作業領域にする。キー項目 / キー項目候補 / グリッドの 3 区分で表示し、キー項目とグリッドセルをテキストボックスで修正して確定できるようにする。

### 変更内容

- 新規 `ResultEditor.vue`（読取結果タブ本体）: `v-expansion-panels`（複数同時展開、初期全展開）で 3 区分を縦に配置
  - キー項目: ラベル（実行時 `field_labels`）+ 確信度 % + 編集可テキストボックス（`raw_fields` の文字列。null は空欄・プレースホルダ「（値なし）」）+ 確信度バー。`visible=false` は非表示。number / date 型は入力中に正規化の可否を行内表示（「正規化値 …」/「数値として解釈できません」等。送信は妨げない）
  - キー項目候補: パターンごとにラベルと一致値のチップ。正規表現は表示しない。0 件は「一致なし」。読取専用
  - グリッド: テーブルごとに「表 n（r 行 × c 列）」と CSS グリッド。全セル（ヘッダ行含む）編集可、`columnHeader` は背景色で区別、セルが無い座標は破線の空枠、横は区分内スクロール
  - 失敗実行はエラー表示のみ
- 新規 Pinia ストア `draft`: 差分のみ保持（`fields: {key: string|null}` / `cells: {"t:r:c": string}`）。元値に戻ったら差分から外す。空欄は null
- `WorkPanel`: 上部に選択中結果のチップ（区分・日時・修正者）。下部固定のアクションバー「変更 N件」+ 破棄 + 確定（N ≥ 1 のときのみ有効）。確定で `POST …/corrections` に差分を送り、応答を選択状態にして履歴へ追加、スナックバー「修正を保存しました」。失敗時はエラー表示（下書き保持）
- 未保存の変更がある状態での履歴選択 / IDP実行 / 文書切替（ヘッダの文書 ID 変更）に確認ダイアログ「未保存の変更を破棄しますか？」、ブラウザ離脱に `beforeunload` 警告
- 人手修正済みの値（保存済み `corrections` と下書き）はテキストボックス左端のアクセント + 鉛筆アイコン（ツールチップ「修正」）で表示
- 実行履歴: 区分「初回 / 再実行 / 修正」と修正者を表示。モデル ID・API バージョンは表示しない
- `document` ストア: `hiddenFieldKeys` を `fieldDefs`（key / label / type / visible）に置き換える
- 新規 `utils/normalize.ts`（クライアント側の正規化ヒント）
- ドキュメント: 新規 `docs/viewer/result-panel.md`、`viewer/README.md`

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 3 区分の描画、`visible=false` の除外、編集での件数増減と元値復帰での減少、正規化ヒント、修正済みマーク、空セル枠がテストで確認されている
- 確定で差分ボディが POST され、応答の行が選択・履歴追加される。アクションバーの活性、破棄確認ダイアログがテストで確認されている
- 実装はモックアップ（`.ignore/mockup/`）と同等の見た目・挙動になっている
- `npm run test` / `npm run lint` / `npm run build` 通過
- `/ponytail:ponytail-review` でレビュー済み

### 関連情報

- 親: #112
- 依存: #114（解析結果 API の分割）、#115（人手修正の保存 API）

