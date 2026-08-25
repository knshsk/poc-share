title:	[Bug] Vuetifyコンポーネント未登録により画面が素のHTMLで描画される
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
number:	23
--
### 事象

ログイン後の画面で、ボタンがスタイルなしのテキストとして表示され、文書ID入力欄（v-text-field）が表示されない。

### 原因

`src/main.ts` の `createVuetify()` に `components` / `directives` を渡しておらず、`<v-btn>` `<v-text-field>` 等が未知のカスタム要素として素通し描画されていた。スロットテキストを持つ要素（ボタン）は文字だけ見え、持たない要素（テキストフィールド）は不可視になる。Viewerスケルトン導入時からの潜在バグで、入力欄の追加により顕在化した。

### 対応

- Vuetify設定を `src/plugins/vuetify.ts` に切り出し、`components` / `directives` を登録
- `main.ts`・テストで同一設定を使用（設定差異による見逃しを防止）
- 回帰テスト: v-text-field が input 要素として描画されることを確認

### 完了条件

- [ ] ログイン後の画面でVuetifyスタイルが適用され、文書ID入力欄が表示される
- [ ] 回帰テストが追加されている
- [ ] 品質ゲート通過

## Comments

