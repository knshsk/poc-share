title:	[Feature] 読み取り結果の項目名をラベル表示に変更
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
number:	83
--
### 目的

WorkPanelの読み取り結果に表示される項目名がQuery Fieldsのキー名（文書種別設定の`fields`->`key`）であり、内部値のためエンドユーザーにわかりづらい。表示ラベル（`fields`->`label`）を表示する。

### 変更内容

- ラベルは変更される可能性があるため、実行時のラベル値を`idp_runs.field_labels`（JSONB、key→label）にスナップショット保存
- idp-runs APIレスポンスに`field_labels`を追加
- WorkPanelは`field_labels`のラベルを表示、無ければキー名フォールバック（label保存前の既存runはキー名表示）

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）

### 完了条件

- [ ] `idp_runs`に`field_labels`カラム追加（マイグレーション）
- [ ] 実行成功時に実行時点のラベルを保存
- [ ] APIレスポンスに`field_labels`が含まれる（既存runはnull）
- [ ] WorkPanelでラベル表示・キー名フォールバック
- [ ] テスト追加（保存・返却・既存run null）

### 関連情報

なし
