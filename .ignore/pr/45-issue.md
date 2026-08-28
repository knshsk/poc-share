title:	[Chore] ブランチ運用ルールにchore区分を追加しISSUEテンプレートを整備
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
number:	45
--
## 目的

ドキュメント整備やCI/CD整備など、アプリのコードに関与しないタスクのブランチ運用が未定義。feature/fixの2系統では吸収できないため、3つ目の区分を新設する。

## 作業内容

- ブランチ規約に `chore/<issue番号>-<簡易説明>` を新設
- ISSUEテンプレート `chore.yml` を追加（整備タスク用）
- PRテンプレートのブランチ規約チェックリストを更新
- GitHubに `chore` ラベルを作成

## 対象コンポーネント

- その他

## 完了条件

- [ ] `.github/ISSUE_TEMPLATE/chore.yml` が追加されている
- [ ] PRテンプレートのチェックリストにchore規約が記載されている
- [ ] `chore` ラベルが作成されている

## 関連情報

なし

## Comments

