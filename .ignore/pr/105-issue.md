title:	[Feature] 書類種別設定のシードを purchase_order / quotation の2種別に変更する
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
number:	105
--
### 目的

現在のシードは `order_project` / `order_route` / `quote` の3種別だが、業務フロー上、注文書はIDP実行後に案件型・ルート型へ分岐するため、IDP実行前の種別として案件/ルートを分けるのは誤り。注文書・見積書の2種別に改める。

### 変更内容

- 新規マイグレーション追加
  - `documents.document_type` を `quote→quotation`、`order_project`/`order_route→purchase_order` に更新
  - `default` テナントの旧3設定を削除し、新2設定を投入
  - downgrade は逆変換（`purchase_order→order_project` は不可逆）
- 新シードのフィールド（Query Fields キーはDIドキュメントに倣いPascalCase）
  - `quotation`（見積書、shareable=true）: `SenderCompanyName`, `ReceiverCompanyName`(visible=false), `OrderDate`(date), `TotalAmount`(number), `DeliveryDate`(date), `DeliveryLocation`, `CustomerContactPersonName`, `QuotationId`
  - `purchase_order`（注文書、shareable=false）: 上記＋`PurchaseOrderId`
- テストfixture（conftest `DEFAULT_CONFIGS`）とAPI/viewerテストの種別コード固定値を新コードに揃える
- `docs/data-model/document-type-configs.md` のシード表を更新

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] `alembic upgrade head` 後、`default` テナントの種別が `purchase_order` / `quotation` の2件になる
- [ ] 既存 `documents` 行の `document_type` が新コードへ移行される
- [ ] API/viewer テスト通過
- [ ] ドキュメント更新済み

### 関連情報

- ADR-0002（注文書は閲覧URL発行不可）
- ADR-0014
- #103
