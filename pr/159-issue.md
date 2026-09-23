title:	[Feature] Viewer 右ペインの再設計・幅の可変化・文書タイトル表示（UI 是正 第2弾・親Issue）
state:	CLOSED

labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	
sub-issues:	shiro-ino/factone-idp-viewer#160, shiro-ino/factone-idp-viewer#161, shiro-ino/factone-idp-viewer#162
sub-issues-completed:	3/3
blocked-by:	
blocking:	
number:	159
--
### 目的

UI 是正 第1弾（#157）で「効いていないスタイル」を直した後の画面を再評価し、右ペイン（読取結果の確認・修正）に残った使いづらさを解消する。

- 確信度が「色付きの % + バー」の二重表現で大半が赤になり、注意喚起として働かない。人手修正済みの項目は「-」表示
- グリッドが入力欄の羅列で、空行も全セルを描画し、6 列表は横スクロールが必須。ヘッダーも入力欄で見分けにくい
- 右ペインの幅が 528px 固定
- 未保存の下書きと保存済みの修正が同じ見た目
- 選択中結果のチップの意味が伝わりにくく、過去の結果を見ていても気付けない
- 画面用語が開発者の語彙（「グリッド」「キー項目候補」「原本DL」）
- ヘッダーの文書 ID バッジが浮いて見える。利用者には文書 ID より原本ファイル名のほうが分かりやすい
- 第1弾の持ち越し（一覧の取得失敗と IDP 実行失敗を区別できない、閲覧URL 一覧の階層）

### 変更内容

設計はローカル管理の spec（`2026-09-18-viewer-right-pane-redesign-design.md`）と実 Vuetify のモック（`.ignore/mockup/editor.html`、リポジトリ管理対象外）に基づく。Sub-issue で分割実施する（順序は C → B → A。互いに独立）。

1. #160 ヘッダーの文書タイトル化（API の文書メタデータ応答に原本ファイル名を追加し、アプリバーに「種別 + 原本ファイル名 + 文書 ID」をタイトルとして表示）
2. #161 右ペイン幅の可変化と上部の改善（境界のドラッグ / キー操作、「表示中」表示と「過去の結果」警告、一覧の取得失敗フラグ、「原本ダウンロード」、閲覧URL 一覧の階層）
3. #162 読取結果エディタの再設計（キー項目の状態表示、候補値の読取専用欄、表組み・空行の畳み・列幅の配分、キャンバス連動時の自動展開、用語「候補値」「表」）

変えないもの: 初期ズーム（ページ全体を表示）、フッター、IDP実行ボタンの位置、Mediator のイベント（追加なし）。配色・ロゴの刷新、結合セルの span 描画、右ペイン幅の保存は対象外。

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 全 sub-issue の PR がマージされている
- 各 sub-issue の画面がモックと同等の見た目・挙動になっている
- `docs/viewer/`・`docs/architecture/viewer.md`・`docs/api/openapi.json`・`docs/data-model/documents.md`・`viewer/README.md` が更新されている

### 関連情報

- #157 / #158（UI 是正 第1弾）
- #112（読取結果の修正機能。右ペインの現行設計）、#116（初期全体表示・フッター・右ペイン幅 528px の決定）

