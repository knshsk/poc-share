title:	[Feature] Viewer: キャンバスのフィットズーム・初期全体表示・フッター・右ペイン幅
state:	CLOSED
labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	shiro-ino/factone-idp-viewer#112
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	116
--
### 目的

原本照合の作業性を上げるため、キャンバスの表示倍率を書類の高さ・全体に合わせられるようにし、初期表示で書類全体を見せる。あわせて右ペイン刷新に備えてペイン幅を広げ、下端の表示領域（フッター）を設ける。

### 変更内容

- `DocumentCanvas`: ズームモード `width`（幅合わせ）/ `height`（高さ合わせ）/ `page`（全体表示）/ `custom`（手動）を導入。初期表示は `page`。ツールバーに幅合わせ / 高さ合わせ / 全体表示のトグル群（選択中を強調、± 操作で非選択）を追加
  - `height` はスクローラの可視高さ（上下 padding・ページ枠の下マージン・枠線を除く）に 1 ページが収まる倍率、`page` は `min(100, height の倍率)`。複数ページ文書は 1 ページ目の寸法で計算
  - `ResizeObserver` で `custom` 以外はコンテナサイズ変化時に再計算
  - ± は 25% 刻み・25〜200% に制限。フィット計算値は制限しない（最小 1）
- `App.vue`: `v-footer app`（高さ 32px）を追加し、ログイン前・認証後・公開閲覧（`/view/{token}`）の全状態で表示。左に「FactONE Viewer」、右にプレースホルダリンク「プライバシーポリシー」「お問い合わせ」「ライセンス」（`href="#"`、遷移先なし）
- `DocumentView`: 右ペイン幅 380px → 528px。2 ペインの高さを `calc(100dvh - var(--v-layout-top) - var(--v-layout-bottom))` で算出する（現在の 64px 固定は app-bar の comfortable 密度と不一致）
- ドキュメント: `docs/viewer/display.md`（ズームモード・フッター・高さ計算）

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 初期表示で書類全体が表示され、3 種のフィットと ± が仕様どおり動く（フィット計算のテスト添付）
- フッターが各状態で描画される（テスト添付）
- 右ペイン幅 528px、ペイン内スクロールが維持されている
- `npm run test` / `npm run lint` / `npm run build` 通過

### 関連情報

- 親: #112
- モックアップ: `.ignore/mockup/`（ローカル管理）

