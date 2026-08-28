title:	[Chore] docs/businessディレクトリ新設（業務フロー・業務要件の格納先整備）
state:	CLOSED
author:	shiro-ino (しろいの)
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
number:	53
--
### 目的

docs配下は技術設計中心（adr/api/architecture/azure/data-model/develop-guide/viewer）で、業務フロー・業務要件を格納する場所がないため、`docs/business/` を新設し、記述規約の整備と初期ドキュメントの格納を行う。

### 作業内容

- `docs/business/README.md` を新規作成（ドキュメント一覧・対象範囲・記述方針を記載）
  - 業務フロー図は Mermaid 記法で記述
  - 確定事項のみ記述し、経緯・却下案は `docs/adr/` に記録（既存方針踏襲）
  - 技術的な実現方式は記述せず `docs/api/`・`docs/data-model/` 等に委ねる
- `docs/README.md` のディレクトリ一覧に `business/` 行を追加
- 初期ドキュメントを格納
  - `factone-concept.md` — FactONE PJ全体構想（目的・案件型とルート型の違い・検証観点・環境構成・全体像）
  - `operation-flow.md` — 業務フロー（レーン定義とK1〜K5・R1・F1〜F4のシーケンス図）
  - `out-of-scope.md` — PoC対象外事項一覧（対象外ID単位、各見出しに簡易説明付き）
  - `data-model.md` — FactONE全体のデータモデル（プレースホルダ、内容は後日追記）
  - `entity.md` — 工程×エンティティ俯瞰図・対応マトリクス（プレースホルダ、内容は後日追記）

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- [ ] `docs/business/README.md` が存在し、ドキュメント一覧・対象範囲・記述方針が記載されている
- [ ] `docs/README.md` に `business/` の説明行がある
- [ ] 初期ドキュメント5件（factone-concept / operation-flow / out-of-scope / data-model / entity）が格納されている

### 関連情報

なし

## Comments

