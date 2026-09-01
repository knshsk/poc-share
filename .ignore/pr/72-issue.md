title:	[Feature] DI読み取り結果へのテーブル領域（明細仮）の追加
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
number:	72
--
### 目的

現在のDI読み取りはQuery Fieldsで取得した書類ヘッダ項目のみ返却しており、実際の書類が持つ明細部の情報が不足している。DIが認識したテーブル領域を明細（仮）として保持・返却し、後続システムで明細情報を利用可能にする。

### 変更内容

- `api/app/services/di.py`: analyze返却型を `AnalysisResult(fields, tables)` に拡張。prebuilt-layout応答の `result.tables` から行列数・セル（行列index・content・kind・ページ番号・polygon座標）を抽出。複数テーブル認識時は全件保持
- `api/app/services/idp_extraction.py`: テーブルは正規化・意味付けせず素通し（明細領域の特定・SAP入力項目との対応付けはSaaS側の責務外）。JSON化ヘルパ追加
- `api/app/models.py` + alembicマイグレーション: `idp_runs.tables` nullable JSONB列追加（失敗実行はNULL、成功でテーブル未検出は空配列）
- `api/app/routers/idp_runs.py`: 成功時にtables保存、POST/GET応答に `tables` 追加
- 横断差分取得IF（#63）は変更なし

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 完了条件

- [ ] DI解析応答のテーブル領域が `idp_runs.tables` に保存される
- [ ] IDP実行POST・履歴GET応答に `tables` が含まれる
- [ ] 複数テーブル認識時に全件返却される
- [ ] 失敗実行はNULL、テーブル未検出は空配列
- [ ] DI応答パース（重点領域）のテストを添付

### 関連情報

- #63（横断差分取得IF: 影響なし）
