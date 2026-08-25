title:	[Feature] 検証用サンプルPDFと作成規約
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
number:	29
--
### 目的

手動検証・営業レビュー共有に使う検証用サンプルPDFを整備する。顧客実データ不使用の制約を構造的に担保するため、サンプルはスクリプト生成とし、作成規約を定める。

### 変更内容

- `docs/develop-guide/samples.md` — サンプル作成規約（実データ由来禁止・ダミー値規則・命名規則）
- `scripts/generate-samples.py` — サンプルPDF生成スクリプト（uvインラインメタデータ+reportlab）
- `samples/` — 生成したサンプルPDF3種をコミット
  - 見積（quote-001.pdf）
  - 注文書・案件型（order-project-001.pdf）
  - 注文書・ルート型（order-route-001.pdf）
  - 注文書は最低抽出項目（得意先コード・品目コード・数量・納期・単価）を含む表形式。確定抽出項目が未確定のため様式は仮置き

### 完了条件

- [ ] スクリプト再実行で同一様式のPDFが再生成できる
- [ ] サンプルに実データ由来の値が含まれない（規約準拠）
- [ ] 作成規約が `docs/develop-guide/samples.md` に記載され、README一覧が更新されている

### 関連情報

- スコープ外: 様式バリエーション・異常系サンプル（テストfixtures側の責務）

## Comments

