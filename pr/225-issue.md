title:	[Bug] 見積書のシードの担当者の項目が、自社の営業担当者ではなく宛先の担当者を指している
state:	CLOSED
labels:	bug
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
number:	225
--
### 事象

`tenant_id`が`default`の見積書の文書種別（`quotation`・`quotation_di`）は、担当者の項目を`CustomerContactPersonName`（ラベル「取引先担当者」）として持つ。`quotation`（CU）には、宛先の会社の担当者を指す説明を付けている。

しかし、見積書に載る担当者は、見積書を発行した自社の営業担当者である。そのため、見積書の2種別では、項目のキー・ラベル・説明が帳票の内容と合っていない。CUは説明に従って宛先の担当者を探すので、営業担当者を取れないか、別の人名を返すおそれがある。

### 再現手順

1. `GET /api/v1/config/document-types/quotation`で、見積書（CU）の設定を取得する
2. `fields`の`CustomerContactPersonName`のラベルと説明を見る
3. `GET /api/v1/config/document-types/quotation_di`でも、同じ項目を見る

### 期待する動作

- 見積書の2種別では、担当者の項目のキーが`SalesPersonName`、ラベルが「営業担当者」になっている。項目の並び順と型（string）、表示有無（表示する）は変えない
- `quotation`（CU）の説明は、見積書を発行した会社の営業担当者を指す。`quotation_di`は、今までどおり説明を持たない
- CUとDIの種別で、ヘッダ項目のキー・ラベル・型・表示有無が同じである。エンジンを切り替えても、外部システムが受け取るキーは変わらない
- 注文書の2種別（`purchase_order`・`purchase_order_di`）は`CustomerContactPersonName`のままである
- 既存の実行結果は、実行時のキーとラベルのまま残る
- downgradeで元のキー・ラベル・説明に戻る

### 実際の動作

手順2で、キーが`CustomerContactPersonName`、ラベルが「取引先担当者」、説明が`Name of the contact person at the addressee company.`になっている。手順3でも、キーとラベルは手順2と同じである。

### 対象コンポーネント（複数選択可）

API（`api` 配下）、ドキュメント（`docs` 配下）

### 環境

ローカル（マイグレーション`f1a6c3e8b2d4`の適用後）

