title:	[Feature] 閲覧用URL Viewer の UI を通常版 Viewer に寄せる
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
number:	149
--
### 目的

閲覧用 URL（`/view/{token}`）で開く画面（`PublicView`）は、素の「ダウンロード」ボタンとページの縦並びだけの最低限の表示になっている。閲覧者（得意先）が見る画面としてページ移動・ズームができず、見た目も認証後の Viewer と揃っていない。

### 変更内容

- `DocumentCanvas` を閲覧用 URL でも使えるようにする（`token` prop の追加、`documentId` の任意化、ダウンロードボタン文言の prop 化）。ページナビ・ズーム・フィット・ダウンロードボタン・スクロール領域を閲覧者にも提供する。読取矩形・右ペインは出さない
- `PublicView` を `DocumentCanvas` 全幅のレイアウトに置き換え、取得中はページスケルトン、無効 URL / ダウンロード失敗は認証後 Viewer と同じエラー state（アイコン + メッセージ）にする。メッセージ文言は現行のまま
- `AppHeader` に `minimal` prop を追加し、公開閲覧ではロゴだけのアプリバーを出す（文書 ID バッジ・種別チップ・ユーザーメニューは非表示）

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- `/view/{token}` でヘッダ（ロゴのみ）・キャンバスツールバー・ダウンロードが表示され、ページ画像の遅延読込（`page_view` 記録の契機）は従来どおり
- `DocumentCanvas`（token 経路）・`AppHeader`（minimal）・`PublicView`（成功 / 無効 URL / ダウンロード失敗）のテストを追加
- `docs/architecture/viewer.md`・`docs/viewer/display.md` を更新

### 関連情報

- ADR-0015（ページ単位の閲覧イベント）。遅延読込の契機を変えないこと
