title:	[Chore] CLAUDE.mdにブランチ運用・開発フローのルールを追記
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
number:	55
--
### 目的

ブランチ運用ルール（ブランチ名規則、Issue起票→ブランチ作成→PRの流れ）が現状リポジトリ外の資料にしか記載されていない。クローンした他メンバーの環境ではエージェントがルールを参照できないため、リポジトリに含まれるCLAUDE.mdへ明文化する。

### 作業内容

CLAUDE.mdに「開発プロセス」節を追記する。

- ブランチ運用（GitHub Flow、main直接push禁止・PR必須、squash mergeのみ）
- ブランチ名規則（feature/fix/chore）と区分基準
- 開発フロー（Issue起票→ブランチ作成→実装→PR→レビュー→マージ）
- 完了条件（品質ゲート通過、重点領域のテスト添付、ADR対象決定時のADR起票）

### 対象コンポーネント（複数選択可）

その他

### 完了条件

- [ ] CLAUDE.mdにブランチ運用・開発フローの節が追記されている
- [ ] リポジトリ外の資料を参照しなくてもルールが把握できる

### 関連情報

なし

## Comments

