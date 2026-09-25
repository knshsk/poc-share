title:	[Feature] 読込中の表示をスピナーに統一し、起動スプラッシュと gzip 配信で低速回線の初回表示を改善する
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
number:	172
--
### 目的

- Keycloak でログインした後、Viewer に戻るとログインカードが一瞬（低速回線では数秒）表示される。`/callback` のトークン交換中も画面の判定が `login` になるため
- 書類画像の取得中は灰色のパルスの紙面、文書情報の取得中はスケルトン（白地に灰色帯のページ型・右ペインの棒）が出て、壊れた画面に見える
- 低速回線（Chrome DevTools の Fast 3G 相当・キャッシュ無し）では、ログイン後の約 11 秒が白画面になる。JS 955KB と CSS 1016KB を無圧縮で配信しており、head の CSS が届くまで描画が止まるため。その後もログインカード → スケルトン → 灰色の紙面と、完成形と食い違う画面が続く。アプリバーが現れるときは `v-main` の上余白のアニメーションで 2 ペインがアプリバーの下に潜り、フッターの上に隙間が出る
- 読込中であることを示す表示（スピナー）に統一して途中の画面を見せないようにし、低速回線での白画面の時間も短くする

### 変更内容

- 起動スプラッシュ: `viewer/index.html` の `#app` にスピナー（素の HTML・インライン CSS。`v-progress-circular` と同じ見た目・同じ位置）を置き、Vue の起動と同時に消す。ビルドでは CSS の `<link>` を `</body>` の直前へ移し、CSS の到着前にスプラッシュを描く（Vue は CSS の適用後に起動する）
- ログイン処理中（状態 `booting` / `callback`）は `vm.screen` を `loading` にし、ロゴだけのアプリバーとスピナーを出す（ログインカードを出さない）
- 共通のスピナー部品 `LoadingSpinner` を作り、スケルトン（`DocumentView` のキャンバス・右ペイン、`PublicView` のキャンバス、`WorkPanel` の本文）と `PageSvg` の灰色のパルスを置き換える。ページのスピナーは表示領域に入って画像を要求したページだけに出す
- スピナーの遅延表示: 0.3 秒未満で終わる読込ではスピナーを出さない。表示中のスピナーの続きは待たずに出す（段階の切替で点滅させない）
- ページ画像の取得失敗を記録し、紙面に「ページを表示できませんでした」を出す（今は読込中の表示が続く）
- API に gzip 圧縮（Starlette の `GZipMiddleware`）を入れる。PNG・woff2（既定）と原本の PDF は圧縮しない
- テストとドキュメント（`docs/viewer/display.md`・`docs/viewer/result-panel.md`・`docs/architecture/viewer.md`・`docs/architecture/api.md`・`docs/azure/README.md`・`viewer/README.md`）

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- ログイン後にログインカード・スケルトン・灰色のパルス・フッター上の隙間が出ない
- 低速回線（Fast 3G 相当）で、`/callback` の読込開始から 1 秒以内にスプラッシュが描かれ、Vue の最初の描画の時点で CSS が適用されている
- 高速回線ではスピナーが一度も出ない
- ページ画像の取得に失敗したページは失敗を表示する
- `Accept-Encoding: gzip` の要求に JS・CSS・JSON が gzip で返り、PNG・PDF は圧縮されない
- テストと pre-commit が通り、ドキュメントが実装と一致している

### 関連情報

- 画面内の操作（実行履歴の選択・閲覧URL タブ・原本ダウンロード）の読込表示、フォントの差し替え、バンドルの縮小、静的ファイルのキャッシュヘッダは対象外（後続の候補）
- ページ画像の遅延読込（ADR-0015）は変えない

