title:	[Bug] 書類種別の選択肢が無いとき、選択欄に英語の「No data available」が表示される
state:	OPEN
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
number:	202
--
### 事象

書類種別の選択肢が1つも無いとき、「文書を登録」タブの選択欄を開くと「No data available」と英語で表示される。画面の他の文言は日本語である。

この文言は、Vuetifyの`v-select`が選択肢の無いときに出す既定の文言（`$vuetify.noDataText`）である。Viewerは`createVuetify`でロケールを指定していないので、Vuetifyの既定の文言は英語になる（`viewer/src/plugins/vuetify.ts`）。選択肢が空になるのは、書類種別一覧の取得に失敗したとき（#190）と、テナントに書類種別が1件も無いときである。

使っている部品と指定を確かめた範囲では、英語のまま出る既定の文言はこの1件である。ただし、閉じるボタン付きの`v-alert`や入力欄の`clearable`のように、Vuetifyが既定の文言を出す機能を今後使うと、その文言も英語になる。

### 再現手順

1. ブラウザの開発者ツールで、`/api/v1/config/document-types`へのリクエストをブロックする
2. URLに`document_id`を付けずにログインする（文書選択ダイアログが自動で開く）
3. 「文書を登録」タブの書類種別の選択欄を開く

### 期待する動作

選択肢が無いことを示す文言が、画面の他の文言と同じく日本語で表示される。

### 実際の動作

選択欄のメニューに「No data available」と表示される。

### 対象コンポーネント（複数選択可）

画面（`viewer` 配下）

### 環境

ローカル（Chromium）、Vuetify 4.1.11。コードの確認による（`viewer/src/plugins/vuetify.ts`の`createVuetify`に`locale`の指定が無い）。

