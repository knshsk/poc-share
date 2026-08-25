title:	[Feature] Viewer画面からのIDP再実行と結果表示
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
number:	39
--
## 目的

IDP誤読時に、社内ユーザーがViewer画面上で原本を確認しながらIDPを再実行し、項目化結果と確信度を確認できるようにする。既存のIDP実行API（POST/GET /api/documents/{document_id}/idp-runs）はバックエンド実装済のため、Viewer側の操作・表示を追加する。

## 変更内容

- `IdpRunPanel.vue` 新規作成し `DocumentView.vue` へ組込
  - 「IDP実行」ボタン: POST /api/documents/{document_id}/idp-runs。実行中はスピナー表示+多重実行防止disabled（DI同期実行=最大120秒想定）
  - 結果表示: 最新実行の項目化結果（normalized_fields）+確信度（confidence）を項目別リスト表示。失敗時はerror_info表示
  - 履歴一覧: GET同エンドポイントで全実行を実行日時降順表示（実行区分・ステータス・日時）。行選択で当該回の結果表示
  - 文書表示時に履歴自動取得（既存実行があれば最新結果を即表示）
- PublicView（外部公開ビュー）は変更なし=再実行操作・結果表示とも社内のみ
- API変更なし・結果矩形オーバーレイなし（実行応答に座標情報なし）

## 対象コンポーネント

- Viewer（フロントエンド）

## 完了条件

- [ ] DocumentViewからIDP再実行を実行でき、項目化結果と確信度が表示される
- [ ] 実行履歴一覧が表示され、過去実行の結果を参照できる
- [ ] 実行失敗時にエラー情報が表示される
- [ ] Vitestで実行・表示・エラー分岐のテストが通過
- [ ] 品質ゲート（lint・型チェック・テスト）通過

## Comments

