title:	[Feature] IDP: WBS原価データの文字化（項目化）対応
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
number:	61
--
### 目的

業務フロー K2-6（`docs/business/operation-flow.md`）では、IDPは見積PDFに加えてWBS原価データの文字化（項目化）を行う（要件 IDP-01）。現実装はPDFのみ受付（`api/app/documents.py` で非PDFは415拒否）で、WBS原価データを扱えない。業務フローを正とし、WBS原価データの受付・項目化を実装する。

### 変更内容

- WBS原価データの受付対応（文書種別の追加、PDF以外の形式受付の検討を含む）
- WBS原価データ向けのクエリセット定義と項目化・正規化・確信度返却
- データモデル・設計ドキュメントの更新（`docs/data-model/idp-runs.md` は「WBS原価データは様式確定まで対象外」としており、様式確定後に本Issueで解消する）

### 対象コンポーネント（複数選択可）

- API（`api` 配下）
- ドキュメント（`docs` 配下）

### 完了条件

- [ ] WBS原価データの様式（ファイル形式・レイアウト）が確定している（本Issueの前提条件）
- [ ] WBS原価データを登録・項目化でき、項目データと確信度を返却できる
- [ ] DI応答パース・異常系のテストを添付している
- [ ] `docs/data-model/` の該当ドキュメントを更新している

### 関連情報

- 要件: `docs/requirements/idp.md` IDP-01
- 業務フロー: `docs/business/operation-flow.md` K2（K2-6）
- 設計: `docs/data-model/idp-runs.md`（様式確定まで対象外の記載）

## Comments

