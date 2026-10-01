title:	[Bug] 本番ビルドで Vuetify のユーティリティクラスが効かず、余白やボタンの色が崩れる
state:	CLOSED
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
number:	176
--
### 事象

`npm run build` の成果物（`npm run preview`・本番配信）で、Vuetify のユーティリティクラス（`pa-*` / `mb-*` / `bg-*` など）が部品の規則に負け、画面の余白や色が崩れる。`npm run dev` では起きない。ログイン画面では、カードの内側の余白と説明文の下の余白が無くなり、「ログイン」ボタンが primary 色の塗りではなく白地に黒文字になる。#172（PR #173）で CSS の `<link>` を `</body>` の直前へ移してから起きている。

原因は、カスケードレイヤーの優先順が stylesheet の並びで決まっていること。レイヤーの優先順は各レイヤー名が文書内で最初に現れた順で決まり、後に現れたレイヤーほど強い。

- Vuetify はテーマの CSS（`bg-primary` / `text-primary` などを含む `vuetify-utilities` レイヤー）を、実行時に `<style id="vuetify-theme-stylesheet">` として `document.head` の末尾へ足す
- ビルドでは `moveStylesheetsToBody`（`vite.config.ts`）が CSS の `<link>` を body の末尾へ移すため、head にあるテーマの `<style>` が `<link>` より前になる。`vuetify-utilities` が最初に現れ、最下位のレイヤーになる
- その結果、ユーティリティクラスが `vuetify-components`（例: `.v-card` の `padding: 0`、elevated のボタンの背景）や `app-reset`（`p` の `margin: 0`）に負ける。競合する規則の無いクラス（`text-primary` など）は効くため、崩れは一部にだけ出る
- dev では Vite が CSS を head の `<style>` として注入し、テーマの `<style>` はその後ろに入るため、正しい順になる

ログイン画面以外でも、ユーティリティクラスが部品や `app-reset` の規則と競合する箇所は同じ理由で崩れる。

### 再現手順

1. `cd viewer && npm run build && npm run preview`
2. ブラウザで preview の URL（`http://localhost:4173/`）を開く
3. ログイン画面を `npm run dev` の表示と見比べる

### 期待する動作

- 本番ビルドの表示が dev と一致する（カスケードレイヤーの順が stylesheet の並びに左右されない）

### 実際の動作

1440×900 のログイン画面で、Chromium の computed style を計測した結果。

- dev: レイヤー順は app-reset → vuetify-core → vuetify-components → vuetify-overrides → vuetify-utilities → vuetify-final。カードの padding 32px、説明文の margin-bottom 24px、ボタンの背景 `rgb(14, 101, 176)`・文字 白
- preview: レイヤー順は vuetify-utilities → app-reset → vuetify-core → vuetify-components → vuetify-overrides → vuetify-final。カードの padding 0、説明文の margin-bottom 0、ボタンの背景 白・文字 on-surface
- preview で `<link>` を head（テーマの `<style>` の前）へ戻すと、dev と同じ値になる

対応方針: `index.html` の head の最初の `<style>` で、`app-reset` と Vuetify の全レイヤー（`vuetify/styles` 冒頭の宣言と同じ順・入れ子）を宣言し、レイヤーの順を stylesheet の並びから切り離す。宣言が Vuetify の宣言と一致すること、アプリと Vuetify の CSS が使うレイヤーをすべて宣言していることをテストで検査する。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 環境

ローカル（Chromium、1440×900）。`main` で確認。

