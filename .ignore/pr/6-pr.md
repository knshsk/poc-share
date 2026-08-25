title:	fix: Issueテンプレートの構文エラーを修正（#5）
state:	MERGED
author:	shiro-ino (しろいの)
labels:	
assignees:	
reviewers:	
projects:	
milestone:	
number:	6
url:	https://github.com/shiro-ino/factone-idp-viewer/pull/6
additions:	2
deletions:	2
auto-merge:	disabled
--
# 概要

Issueテンプレート2ファイルのYAML構文エラーを修正し、テンプレートが適用されるようにします。

## 関連Issue

Closes #5

## 変更内容

- `bug.yml`・`feature.yml` の `description:` の値を引用符で括った（値中の「ブランチ: 」のコロン+空白がYAMLマッピングとして誤解釈されていた）

## 確認事項

### 共通

- [x] ブランチ名が規約に従っている（`feature/<issue番号>-<簡易説明>` または `fix/<issue番号>`）
- [ ] pre-commit のチェックがすべて通過している（※pre-commit は未導入のため対象外。js-yamlで3ファイルのパース確認済み）
- [x] 影響する設計ドキュメント（`docs/` 配下）を更新した、または更新不要である

### テスト

- [ ] `uv run pytest` / `npm run test` をローカルで実行し、すべて通過している（※テスト基盤は未導入のため対象外）
- [x] 重点テスト領域に変更がある場合、対応するテストを追加・更新した（変更なし）

### API契約

- [x] Pydanticモデルに変更がある場合、TS型を再生成し差分をコミットした（変更なし）
- [x] 外部公開契約（Power Automate向けIF）に影響する場合、`docs/api` のOpenAPIエクスポートを更新した（影響なし）

### アーキテクチャ

- [x] アーキテクチャ上の意思決定を含む場合、ADR（`docs/adr/`）を作成・更新した（該当なし）

## 動作確認方法

1. マージ後、Issues → New issue でテンプレート2種（不具合報告・仕様変更/機能追加）が選択肢に表示されること
2. 空白Issueが作成できないこと（config.yml）

## 備考

なし

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Comments

