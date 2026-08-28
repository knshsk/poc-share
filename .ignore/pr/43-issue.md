title:	[Bug] シェルスクリプトに実行権限がなく、クローン後に手動でパーミッション変更が必要
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
number:	43
--
### 事象

リポジトリ内のシェルスクリプトに git 上の実行権限（100755）が付いておらず、クローン直後にそのまま実行できない。手動で `chmod` する必要がある。

調査の結果、`scripts/local-stack/init-keycloak-db.sh` が git モード 100644 でコミットされていた（他の4本は 100755）。また、今後追加されるスクリプトが同様に実行権限なしでコミットされることを防ぐ仕組みがない。

### 再現手順

1. リポジトリをクローンする
2. `ls -l scripts/local-stack/init-keycloak-db.sh` を確認する
3. `./scripts/local-stack/init-keycloak-db.sh` を実行する

### 期待する動作

- すべての `*.sh` が実行権限 755 でクローンされ、そのまま実行できる
- 実行権限のないスクリプトはコミット時に検知・拒否される

### 実際の動作

`scripts/local-stack/init-keycloak-db.sh` のパーミッションが 644 のため、実行時に以下となる:

```
bash: ./scripts/local-stack/init-keycloak-db.sh: Permission denied
```

### 対象コンポーネント

その他（scripts / 開発環境）

### 環境

ローカル開発環境（Linux / WSL2）。git はファイルの実行ビットを保存するため、100755 でコミットされていればクローン後も維持される。

## Comments

