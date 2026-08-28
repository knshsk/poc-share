title:	[Chore] 社内閲覧イベントを履歴化しない方針の確定化（ドキュメント・ADR）
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
number:	51
--
### 目的

社内（認証済み）閲覧イベントの履歴化要否が未確定のため、view_events の記録対象外を「仮置き」として運用してきた。ルート型注文書を含む社内閲覧イベントは履歴化しないことが決定したため、仮置きを確定に昇格し、決定を ADR として記録する。

### 作業内容

- `docs/data-model/view-events.md` の社内表示非記録の仮置き表記を確定に更新
- ADR-0012「社内閲覧イベントを履歴化しない」を起票（accepted）
- コード変更なし（現行実装は既に非記録であり挙動不変）

### 対象コンポーネント（複数選択可）

ドキュメント（`docs` 配下）

### 完了条件

- [ ] view-events.md から当該の仮置き表記が解消されている
- [ ] ADR-0012 が背景・決定・代替案・影響を含めて追加されている
- [ ] 品質ゲート通過

## Comments

