title:	[Feature] 文書表示（ページ画像配信・SVGオーバーレイ・原本DL）
state:	CLOSED
author:	shiro-ino (しろいの)
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
number:	21
--
### 目的

文書IDから対象PDFをViewerで表示できるようにする。PDFページのオンデマンドPNG変換とAPIプロキシ配信、SVGオーバーレイ表示の器、原本PDF配信を整備する。

### 変更内容

API（すべてJWT必須。テナント不一致・存在しない文書IDは404）:

- `GET /api/documents/{document_id}` — 文書メタデータ
- `GET /api/documents/{document_id}/pages` — ページ数+各ページ寸法（インチ単位。SVG viewBox整合用）
- `GET /api/documents/{document_id}/pages/{page}/image` — pypdfium2によるオンデマンドPNG変換・配信
- `GET /api/documents/{document_id}/pdf` — 原本PDF配信（DL用）
- PDF変換はレンダリングアダプタとして分離
- Blobアダプタへ読出（load）を追加

Viewer:

- 文書表示画面（URLクエリ `?document_id=` 受領。案件ID・ルートIDは要求しない）
- SVG表示コンポーネント（viewBox=ページ寸法、画像はfetch+blob URL。読取矩形rectはprops受けの器のみ）
- 原本DLボタン

テスト（厚くする領域）:

- ページ寸法・viewBox整合の座標系テスト
- 異常系: 存在しない文書ID・ページ範囲外・破損PDF（`api/tests/fixtures` 初活用）

### 完了条件

- [ ] 登録済み文書のページ画像・原本PDFがAPI経由で取得できる
- [ ] Viewer画面で文書IDを指定してページ表示・原本DLができる
- [ ] 異常系（存在しないID・範囲外ページ・破損PDF）がテストで担保されている
- [ ] pre-commit・pytest・Vitest・API契約差分チェックすべて通過

### 関連情報

- スコープ外: 閲覧・DLイベント記録、DI読取矩形の実データ表示、閲覧URLトークン

## Comments

