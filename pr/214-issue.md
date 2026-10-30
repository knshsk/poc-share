title:	[Feature] 抽出エンジンの境界をエンジンに依存しない形にし、Document Intelligenceをその上に載せ替える
state:	OPEN
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
number:	214
--
### 目的

読取エンジンを差し替えられるように、エンジンに依存しない境界と、実行ログ・APIの形を先に入れる。

今の境界は、Query Fieldsのキー名だけを受け取る。実行ログとAPIは、DIの出力の形をそのまま持っている。項目値を並列の5列に分けて保存し、テーブルを`tables`、DIのAPIバージョンとモデルIDを`di_api_version` / `di_model_id`として持つ。2本目のエンジンを加える前に、これらをエンジンに依存しない形に改める。

### 変更内容

- 文書種別設定に`engine`（抽出エンジン）を追加する。値は`azure-di`で、PUTで省略すると`azure-di`になる
- 文書種別設定の項目に`description`（エンジンへ渡す項目の説明）を追加する。省略でき、指定するときは1〜1024文字とする
- `engine`が`azure-di`の種別は、項目を20件までとする（Query Fieldsの上限）。超える設定の保存は、422 `validation-error`にする
- 抽出エンジンの境界の入力を、キー名の列から文書種別設定のスキーマに変える。DIは今までどおり、prebuilt-layoutにQuery Fieldsを付けて呼ぶ
- 実行ログの項目値の5列（raw_fields / normalized_fields / confidence / field_labels / field_regions）を、項目ごとの値をまとめた`fields`の1列にする。`tables`は`layout_tables`に改名し、`di_api_version` / `di_model_id`は`engine` / `engine_info`に置き換える。既存の実行ログは、マイグレーションで移行する
- APIの応答の`tables`を`layout_tables`に、`di_api_version` / `di_model_id`を`engine` / `engine_info`に改める。修正のボディの`table_cells`は`layout_table_cells`に改める。外部連携システムは未接続のため、v1内の破壊的変更として入れる（ADR-0017）
- Viewerを同じPRで新しい名前に追従させる。読取結果・修正・ハイライトの挙動は変えない

### 対象コンポーネント（複数選択可）

API（`api` 配下）、画面（`viewer` 配下）、ドキュメント（`docs` 配下）

### 完了条件

- 既存の実行ログが新しい形に移行され、DIの行はdowngradeで元の列に戻せる
- 実行・詳細・最新・修正のAPIが新しい名前で動き、`verbose`の有無によるキーの出し分けが今と同じである
- 項目が21件のDI種別の設定は保存できず、20件なら保存できる
- Viewerの読取結果・修正・キャンバスとの相互ハイライトが、今と同じに動く
- 抽出エンジンを文書種別ごとに選ぶ構成のADRを起票し、データモデル・アーキテクチャ・Viewerのドキュメントを更新している

### 関連情報

- 親: #213
- ADR-0017・ADR-0019・ADR-0022

