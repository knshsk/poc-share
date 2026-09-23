title:	[Feature] Viewer の文字サイズ・フォント・ボタン配置などの UI 不整合を是正する（UI 是正 第1弾）
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
number:	157
--
### 目的

Viewer の UI について、以下の不具合に対応する。
- 意図したスタイルが実際には効いていない
- コンポーネント構成を変えずに直せる不整合

調査で発覚した以下の課題は、本 Issue 反映後の見た目で再評価してから別 Issue で扱う。
- 右ペインの再設計（キー項目の密度・確信度表現・グリッドの表組み化など）

調査で確認した主な事実:

- Vuetify 4 には `text-caption` / `text-body-1` / `text-body-2` / `text-h6` が存在せず（MD3 名に移行済み）、`viewer/src` の 28 箇所が無効クラスになっている。ラベル・確信度・読取値・フッターなどの補助情報が 16px で描画され、入力値（14px）より大きい
- ダイアログ・メニュー・ツールチップ・スナックバーは `body > .v-overlay-container` に描画されるため `.v-application` のフォント指定が届かず、Roboto（環境依存のフォールバック）になっている
- キャンバスのフィット 3 ボタンは幅 20px（他のアイコンボタンは 40px）で 1 つの塊に見える
- アプリバーのロゴは左端から 5px、文書 ID バッジは中央付近に浮いている

### 変更内容

1. 全体フォントを Vuetify の CSS 変数 `--v-font-body` で指定し、`.v-application { font-family … !important }` を廃止する（オーバーレイを含む全域を IBM Plex Sans JP にする）
2. タイポグラフィクラスを MD3 名へ置換する（`text-caption`→`text-body-small`、`text-body-2`→`text-body-medium`、`text-body-1`→`text-body-large`、`text-h6`→`text-title-large`）
3. ヘッダー: 文書 ID バッジと種別チップをロゴの右隣（左寄せ）へ移し、ロゴの左余白を 16px にする
4. 操作の優先度: 「確定」「破棄」を通常サイズにする。「IDP実行」は outlined を基本とし、一覧取得済みで実行が 1 件も無いときだけ塗りにする（位置は変えない）
5. 閲覧URL タブ: 状態チップ「失効」→「失効済み」（グレー）、操作ボタンと確認ダイアログの実行ボタン「失効」→「失効させる」
6. 実行結果が無いとき、「実行結果がありません」の下に IDP 実行への案内文を追加する
7. 入力欄の見た目を `vuetify.ts` の `defaults`（`VTextField` / `VSelect`: outlined + compact）で統一し、各コンポーネントの重複 props を削除する
8. 等幅フォントの使用箇所を絞る（日時・選択中結果チップ・ページ数・ズーム% は UI フォント + `tabular-nums`。文書 ID・発行 URL・キー項目候補は等幅を維持）
9. キャンバスのツールバー: フィット 3 ボタンから `icon` 指定を外して幅を確保し、ページ送りは複数ページ文書のときだけ表示する

変えないもの: 初期ズーム（ページ全体を表示）、フッター、IDP実行ボタンの位置、`data-testid`、Mediator のイベントと vm の型。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] 上記 9 項目が反映されている
- [ ] 旧タイポグラフィクラスの再混入を検出するテストを追加している
- [ ] 変更した表示条件・文言（IDP実行の見た目、空状態の案内、閲覧URL の文言、1 ページ文書のページ送り非表示、入力欄の既定値）のテストを追加・更新している
- [ ] `npm run test` / `npm run lint` / `npm run format:check` / `npm run build` と pre-commit が通過している
- [ ] 実画面で、オーバーレイ内のフォントが IBM Plex Sans JP、ラベル・フッターが 12px、フィット 3 ボタンの幅が 32px 以上、文書 ID バッジがロゴの右隣にあることを確認している
- [ ] `docs/viewer/display.md` / `docs/viewer/result-panel.md` / `viewer/README.md` を更新している

### 関連情報

- #116（初期全体表示・フッターの決定。本 Issue では変更しない）
- #149（公開閲覧の UI。ヘッダー・ツールバーを共用するため同じ是正が反映される）

