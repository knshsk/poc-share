title:	[Bug] Issueテンプレートが構文エラーで適用されない
state:	CLOSED
author:	shiro-ino (しろいの)
labels:	bug
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
number:	5
--
## 事象

Issue作成時にテンプレート（不具合報告・仕様変更/機能追加）が選択肢に表示されず、GitHubのテンプレート検証で以下の構文エラーが表示される。

`.github/ISSUE_TEMPLATE/bug.yml`:
`YAML syntax error: (): mapping values are not allowed in this context at line 2 column 34. Learn more about this error.`

`.github/ISSUE_TEMPLATE/feature.yml`:
`YAML syntax error: (): mapping values are not allowed in this context at line 2 column 39. Learn more about this error.`

## 再現手順

1. リポジトリの Issues → New issue を開く
2. テンプレート選択肢が表示されない
3. Settings のテンプレート検証で上記エラーを確認

## 期待する動作

不具合報告・仕様変更/機能追加のテンプレートフォームが選択できる。

## 実際の動作

テンプレートが適用されず、構文エラーが表示される。

## 原因

両ファイル2行目の `description:` の値が引用符なしで、値中に「ブランチ: fix/...」のコロン+空白を含むため、YAMLのマッピングとして誤解釈される。

## 対処

`description:` の値を引用符で括る。

## Comments

