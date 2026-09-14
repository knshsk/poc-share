title:	[Chore] 全体設計の更新に伴い docs/business 配下のドキュメントを更新する
state:	CLOSED
labels:	chore
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
number:	109
--
### 目的

FactONE全体設計の更新（レーン定義・工程・データモデル・PoC対象外事項の見直し）に追随し、`docs/business` 配下のリポジトリ資料を現行の全体設計へ揃える。

### 作業内容

- `operation-flow.md`
  - レーン定義をテーブルから箇条書き形式へ変更
  - K1〜K5・R1・F1〜F4 のシーケンス図を工程表（工程番号・レーン・処理・接続先・通信手段・備考）へ置換
  - K③-2 の通信手段を `URLパラメータ指定` に確定、K②-9 のWBS記載を削除
- `data-model.md`
  - FactONE全体の論理モデル（モデル一覧・エンティティ項目一覧・リレーション一覧）を新規記載
  - 元資料で廃棄扱いのエンティティ（マスタ系・納入日程行・型区分判定ログ・IDP実行ログ等）と ER 図を除去
  - 「※付きは正本が外部システム・本書未定義」「番号は元資料準拠、欠番は廃棄」を明記
- `out-of-scope.md`
  - 全項目の構成を統一（対象外の範囲／PoCでの実装／PoC結果の留保／対象外の理由／本番扱い）、読み方を冒頭に追記
  - OUT-21（マスタのDD実体化）・OUT-22（原価積算データ）を追加、OUT-19 をモデル対象外表記に修正
- `entity.md` を削除（`data-model.md` へ統合）
- `README.md` のドキュメント一覧・記述方針を更新（工程表形式、業務側論理モデルと `../data-model/` 物理モデルの役割分担）

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- [ ] `docs/business` 配下の4ファイル更新と `entity.md` 削除がマージされる
- [ ] `docs/business` 内のリンク・参照ID（OUT-xx／エンティティID）に切れがない
- [ ] pre-commit 通過

### 関連情報

- 本Issueは業務ドキュメントの更新のみ。以下は別途対応する
  - `docs/requirements`・`docs/adr` の工程番号表記統一（K5-5 → K⑤-5、「各シーケンス」→「工程表」）
  - ADR-0002 の superseded 化
  - `docs/data-model/idp-runs.md` 抽出方式節の更新

