title:	[Feature] Viewer画面から文書登録できるようにする（定型外フロー向け）
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
number:	99
--
### 目的

業務フローでは所定領域に格納されたPDFをPower Automateが検知し文書登録APIを呼び出す前提だが、これに当てはまらない定型外フローが存在する。Viewer画面から直接文書登録できる手段を設ける。

### 変更内容

- `DocumentView` の empty state（文書ID未指定）で選択ダイアログを表示し、「文書IDを指定して閲覧」/「新規登録」を選ばせる
- 新規登録フォーム（`DocumentRegisterForm`）: 文書ID（ユーザ入力）・書類種別（`GET /api/config/document-types` の一覧から選択）・PDFファイル
- 送信先は既存の `POST /api/documents`（API変更なし）。成功（201/200）時はその文書IDを自動表示
- エラー表示: 409（別内容で登録済み）/415/422 等は Problem Details の内容を表示し再試行可
- 誤登録時のフォロー手段（取消・差替）はPoCではスコープ対象外

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] empty state で「閲覧」「新規登録」を選択できる
- [ ] 登録フォームから `POST /api/documents` を呼び出し、成功時に登録文書が表示される
- [ ] 異常系（409/415/422）でエラー表示され再試行できる
- [ ] Vitest テスト追加（正常・異常系）
- [ ] `docs/requirements/viewer.md` / `docs/viewer/display.md` 更新

### 関連情報

- 既存API: `POST /api/documents`（冪等・Power Automate連携用）
- OUT-07（DD登録は日次連携）とは別レイヤの話（IDP SaaS への登録経路）
