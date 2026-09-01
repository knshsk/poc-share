title:	[Chore] viewer/src のタイプベースディレクトリ再構成と viewer/README.md 作成
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
number:	70
--
### 目的

viewer 側のディレクトリ構成に明確な準拠パターンがなく、配置判断基準が未定義（`viewer/` ディレクトリがほぼ全 UI を含む「その他」化）。API 側が FastAPI 公式「Bigger Applications」準拠を明文化しているのに対し、viewer 側は対応するドキュメントもない。create-vue 慣習のタイプベース構成に揃え、配置指針を明文化する。

### 作業内容

- `src/auth/store.ts` → `src/stores/auth.ts` へ移動
- `src/viewer/DocumentView.vue`・`PublicView.vue` → `src/views/` へ移動
- `src/viewer/IdpRunPanel.vue`・`ViewUrlPanel.vue`・`PageSvg.vue` → `src/components/` へ移動
- import パスの修正（src / tests）
- `viewer/README.md` を作成し、ディレクトリ構成と配置指針を記載（api/README.md と対称）

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 完了条件

- [ ] `src/` がタイプベース構成（`api/` `components/` `plugins/` `stores/` `views/`）になっている
- [ ] `npm run test` が全件通過
- [ ] pre-commit 通過
- [ ] `viewer/README.md` に構成と配置指針が記載されている

### 関連情報

なし（ロジック変更なしのファイル移動のみ）
