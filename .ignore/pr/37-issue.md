title:	[Feature] IDP実行API（Document Intelligenceアダプタ+実行ログ）
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
number:	37
--
## 目的

登録済文書に対しDocument Intelligence（prebuilt-layout+Query Fields）で項目抽出を実行し、抽出値・項目別確信度を返却・履歴記録できるようにする。Viewerからの再実行や外部連携の基盤となるIDP中核機能。

## 変更内容

- `idp_runs`テーブル追加（追記型。実行区分・処理状態・生値/正規化値/確信度JSON・エラー情報を記録。再実行で過去結果を上書きしない）
- Document Intelligence境界アダプタ（prebuilt-layout+Query Fields、同期応答）
- パース・正規化・確信度分離処理（純粋関数として分離、テスト対象）
- `POST /api/documents/{document_id}/idp-runs` — IDP実行（M2M認証）。実行区分（初回/再実行）はサーバ側で既存実行有無から自動判定
- `GET /api/documents/{document_id}/idp-runs` — 実行履歴一覧
- 文書種別別クエリセット定義（注文書: 得意先コード・品目コード・数量・納期・単価。見積: サンプル様式の主要項目）
- migration・OpenAPI再生成・データモデルドキュメント追加

## 対象コンポーネント

IDP（API）

## 完了条件

- [x] 登録済の見積・注文書PDFに対しIDP実行でき、抽出値と項目別確信度がJSONで返る
- [x] 同一文書IDの再実行が許容され、履歴が追記される（初回/再実行の区分記録）
- [x] DI失敗時も実行レコードが記録される（処理状態=失敗+エラー情報）
- [x] 異常系（文書なし・他テナント文書ID・未認証）が適切なステータスで応答
  - 当初「非対応種別」を含めていたが、全文書種別にクエリセットを定義したため該当経路なし（WBSは文書種別として存在しない）。文言を実態に合わせ修正
- [x] パース・正規化・確信度分離のテスト添付、品質ゲート通過

## 関連情報

- 仮置き: 確定抽出項目が未確定のためクエリセットはサンプルPDF様式で暫定。同期/非同期方式が未確定のため同期応答で暫定（タイムアウトは設定値）
- WBS原価データは様式確定まで対象外
- 関連ADR: 0004（境界アダプタ）、0005（追記型ログ・生値/正規化値分離）

## Comments

