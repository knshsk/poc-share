title:	[Feature] Viewerスケルトン（Vite+Vue+Vuetify+Pinia起動・Vitest基盤）
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
number:	9
--
### 目的

Viewer（フロントエンド）開発の土台を整備する。以降の画面実装・認証連携が乗る最小の骨格を先に確立し、起動・テストの各基盤を確定させる。

### 変更内容

- `viewer/` に Vite + Vue 3 + TypeScript プロジェクトを新規作成（npm管理、Node.js 24 LTS）
- Vuetify・Pinia を導入しプラグイン登録
- 最小の `App.vue`（アプリ名表示程度。画面実装は含めない）
- Vitest + Vue Test Utils によるテスト基盤（App レンダリングのテスト）
- `docs/develop-guide/viewer.md` — セットアップ・起動・テスト実行手順
- `.gitignore` へ Node 系エントリ追記（`node_modules/`・`dist/` 等）

### 完了条件

- [ ] `npm run dev` で開発サーバが起動し画面が表示される
- [ ] `npm run build` が成功する
- [ ] `npm run test` が通る（App レンダリングのテスト含む）
- [ ] 手順が `docs/develop-guide/viewer.md` に記載されている

### 関連情報

- スコープ外: 画面実装、OIDC認証、eslint/prettier 設定、OpenAPI 型生成、Phosphor Icons 導入

## Comments

