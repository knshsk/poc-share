title:	[Chore] 正規表現パターン抽出のタイムアウト対策（re2 等）の方針決定
state:	OPEN
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
number:	131
--
### 目的

文書種別設定 `patterns[].regex` は Python `re` で DI 全文テキストに適用される。`re` にはタイムアウトが無く、破滅的バックトラックを起こす正規表現が登録されると IDP 実行が長時間化する（`api/app/routers/document_type_configs.py` の `ponytail:` コメント、設計 §10「本番前に検討」）。

### 作業内容

方針を決めてから実装する。候補:

- A. `google-re2`（`re2` パッケージ）へ差し替え（バックトラックなし・線形時間。後方参照・先読み等の非対応構文は登録時に 422）
- B. 登録時に静的検査（ネストした量指定子の検出）+ 実行時のタイムアウト（別プロセス / `regex` パッケージの `timeout`）
- C. 現状維持（設定は管理者のみが登録できる前提を文書化）

### 対象コンポーネント（複数選択可）

API（`api` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 方針が決まり、A / B の場合は ADR を作成
- 破滅的バックトラックを起こす正規表現で IDP 実行が有界時間で終わる（または登録時に拒否される）ことのテスト
- `docs/data-model/document-type-configs.md` 更新、`ponytail:` コメント削除

### 関連情報

- #107（正規表現パターン抽出）、設計 §10

