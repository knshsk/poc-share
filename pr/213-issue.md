title:	[Feature] 書類の読取エンジンを文書種別ごとに選べるようにする
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
number:	213
--
### 目的

書類の読取（項目の抽出）は、Azure AI Document Intelligence（以下DI）の`prebuilt-layout`にQuery Fieldsを付けたものに固定されている。この構成には次の課題がある。

- DIが使えなくなったときの代替手段がない。サービスの廃止、日本リージョンでの提供終了、価格改定などが起こりうる
- 明細は、レイアウト認識の結果をテーブル領域として返すだけで、列や行の意味を判断していない。どの列が品名や数量にあたるかの判断は、呼出側に任せている。セル単位の信頼度もない
- Query Fieldsはキー名しか指定できず、項目に説明を付けられない。指定できる項目は20件までである

抽出をDI固有の形から切り離し、文書種別ごとに読取エンジンを選べるようにする。2本目のエンジンとして、Azure AI Content Understanding（以下CU）を加える。

### 変更内容

次の4つの子Issueに分けて進める。

- #214: 抽出エンジンの境界をエンジンに依存しない形にし、DIをその上に載せ替える
- #215: CUのアダプタを加え、明細をオブジェクトのリストとして取れるようにする
- #216: Viewerで明細を表示・修正できるようにする
- #221: シードの注文書・見積書の読取エンジンをCUにし、DIの種別を別の種別コード（`purchase_order_di`・`quotation_di`）で残す

変更が大きいため、4つがそろうまで`main`には取り込まない。子IssueのPRは統合ブランチ`feature/213-engine-neutral-extraction`に取り込む。統合ブランチを検証環境で確かめてから、`main`へ取り込む。

### 対象コンポーネント（複数選択可）

API（`api` 配下）、画面（`viewer` 配下）、IaC（`bicep` 配下）、スクリプト（`scripts` 配下）、ドキュメント（`docs` 配下）

### 完了条件

- 子Issue（#214・#215・#216・#221）がすべて完了している
- 統合ブランチを検証環境にデプロイし、DIとCUの文書種別で、抽出・確認・修正ができることを確かめている
- 統合ブランチを`main`へ取り込み、`main`を検証環境へデプロイし直している

### 関連情報

- ADR-0004（外部システム固有の処理を境界アダプタとして分離する）
- ADR-0017（外部連携システムが接続するまでは、v1内の破壊的変更を許容する）


