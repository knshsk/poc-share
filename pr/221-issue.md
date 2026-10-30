title:	[Feature] シードの注文書・見積書の読取エンジンをContent Understandingにし、DIの種別を別のコードで残す
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
number:	221
--
### 目的

シードの文書種別（`purchase_order`・`quotation`）は、DIの`prebuilt-layout`とQuery Fieldsで抽出している。明細はレイアウト認識の表でしか返せず、どの列が品名や数量かの判断を呼出側に任せている。テストデータの注文書・見積書10枚で比べたところ、ヘッダ項目の正解数はCUが64/68、DIが47/68だった。CUは明細を行ごとの値として返し、明細の行数は全件で正解と一致した。

シードの文書種別の読取エンジンをCUにし、明細を項目として返す。DIの文書種別は、比較と切り戻しのために別の種別コードで残す。

### 変更内容

- `tenant_id`が`default`の文書種別`purchase_order`・`quotation`を、`purchase_order_di`（表示名「注文書（DI）」）・`quotation_di`（表示名「見積書（DI）」）に改名する。パターン・閲覧URL発行可否は変えない。項目は、見積書に版数を足すほかは変えない。この種別を参照する文書の`document_type`も書き換える
- CUの文書種別`purchase_order`（注文書）・`quotation`（見積書）をシードとして追加する
  - ヘッダ項目は、DIの文書種別と同じキー・ラベル・型・表示有無とする。外部システムが受け取るヘッダ項目のキーは変わらない
  - 明細は、array型の項目`LineItems`（明細）で取る。プロパティは`ItemCode`（品番）・`ItemName`（品名）・`Quantity`（数量）・`UnitName`（単位）・`UnitPrice`（単価）・`LineAmount`（金額）とする
  - 項目とプロパティの説明（`description`）は、英語で書く
  - パターン（登録番号）と閲覧URL発行可否は、DIの文書種別と同じにする
- 見積書の2種別（`quotation`・`quotation_di`）に、版数`RevisionNumber`（string型）を加える。帳票の版数は「第2版」のように書かれ、number型の正規化規則では数値にできないためである。CUとDIでヘッダ項目のキーが同じという方針は保つ
- CUの文書種別では、`layout_tables`が空配列になる。明細は`fields.LineItems.items`で返る
- downgradeでは、CUの文書種別を参照する文書があれば止める

### 対象コンポーネント（複数選択可）

API（`api` 配下）、ドキュメント（`docs` 配下）

### 完了条件

- マイグレーション後、`default`テナントに、CUの`purchase_order`・`quotation`と、DIの`purchase_order_di`・`quotation_di`の4種別がある
- 見積書の2種別（`quotation`・`quotation_di`）が、版数`RevisionNumber`を持つ
- 既存の`purchase_order`・`quotation`の文書が、`_di`の文書種別を参照している
- CUの文書種別で登録した文書をIDP実行すると、ヘッダ項目がDIの文書種別と同じキーで返り、明細が`LineItems`で返る
- downgradeで元の2種別に戻る。CUの文書種別を参照する文書があるときは、downgradeが止まる
- `docs/data-model/document-type-configs.md`のシードデータの記載が、4種別になっている

### 関連情報

- 親: #213
- ADR-0029（文書種別の単位でエンジンを選ぶ）に、シードの読取エンジンを追記する


