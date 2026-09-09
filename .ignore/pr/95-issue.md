title:	[Feature] 原本PDFダウンロード時のファイル名をアップロード時のオリジナルファイル名にする
state:	OPEN
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
number:	95
--
### 目的

Viewer（閲覧用URLでアクセスした画面も含む）から原本PDFをダウンロードした際、ファイル名が `{document_id}.pdf` 固定になっている。利用者がアップロード時のファイル名で受け取れるようにする。

### 変更内容

- `documents` テーブルに `original_filename`（varchar(255), NULL可）を追加。登録API（`POST /api/documents`）で multipart の `filename` の basename を保存する（空・未指定は NULL）
- 原本DL API 2経路（`GET /api/documents/{document_id}/pdf`, `GET /view/{token}/pdf`）の `Content-Disposition` を RFC 6266/5987 形式（`filename` + `filename*=UTF-8''...`）で返す。`original_filename` が NULL の既存文書は従来どおり `{document_id}.pdf`
- Viewer（DocumentView / PublicView）は `Content-Disposition` からファイル名を取り出して保存名に使う

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] 日本語を含むオリジナルファイル名でダウンロードした際、両経路でそのファイル名で保存される
- [ ] `original_filename` が無い文書は `{document_id}.pdf` で保存される
- [ ] API・Viewer のテスト追加
- [ ] `docs/data-model/documents.md`・`docs/api/openapi.json` 更新

### 関連情報

なし
