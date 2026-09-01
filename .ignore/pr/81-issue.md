title:	[Feature] 文書種別のテナント別設定値化
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
number:	81
--
### 目的

マルチテナントSaaSとして、テナントごとに異なる文書種別を設定できるようにする。現在は文書種別（Query Fields・閲覧URL発行可否）がAPIのソースコードにハードコードされており、テナント別の差分を表現できない。

### 変更内容

- `document_type_configs` テーブルを新設（テナント別・種別別の設定。フィールド定義はJSONB配列）
- 文書種別ごとに Query Fields のキー・ラベル・型（string/number/date）、閲覧URL発行可否（`shareable`）を設定可能にする
- 設定CRUD API（`/api/config/document-types`）を追加
- `DocumentType` enum と `documents` テーブルのCHECK制約を廃止し、種別コードを自由値化（検証は設定テーブル参照）
- 正規化をキー名ハードコードから型駆動へ変更
- 既存3種別（quote / order_project / order_route）はシードデータ化

### 対象コンポーネント（複数選択可）

API（`api` 配下）、画面（`viewer` 配下）、ドキュメント（`docs` 配下）

### 完了条件

- [ ] 設定CRUD APIでテナントの文書種別を作成・取得・更新・削除できる
- [ ] IDP実行のquery_fieldsと正規化が設定テーブル由来になる
- [ ] 閲覧URL発行可否が `shareable` 設定で制御される
- [ ] 文書登録時、未登録種別は422で拒否される
- [ ] 使用中種別の削除は409で拒否される
- [ ] 既存3種別のシードで現行動作が維持される（テスト全通過）
- [ ] ADR起票（ADR-0002の実現手段変更）

### 関連情報

- ADR-0002（文書種別の共有可否）
