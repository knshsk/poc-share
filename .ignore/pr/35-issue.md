title:	リポジトリ外資料由来のID参照をコード・ドキュメントから除去
state:	CLOSED
author:	shiro-ino (しろいの)
labels:	
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
number:	35
--
## 背景

リポジトリに含まれない作業資料で採番された要件IDへの参照が、ソースコードのコメント・docstringおよびdocs配下の文書に残存している。リポジトリ内だけでは意味を辿れないため除去する。

## 対応

- コメント・docstring・docs中の当該ID参照を除去し、文意が通るよう修正
- IDが説明の実体を担う箇所は説明文へ展開
- リポジトリ内に実在する資料（ADR・Issue/PR番号）への参照は維持

## 対象

- api/app/（models.py・documents.py・view_urls.py）
- api/tests/（test_view_url.py・test_view_events.py）
- docs/data-model/・docs/viewer/

## Comments

