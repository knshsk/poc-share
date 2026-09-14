title:	[Bug] DI 解析のタイムアウト時に失敗実行が記録されず 500 になる
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
number:	119
--
### 事象

Document Intelligence の解析が `FACTONE_DI_TIMEOUT_SECONDS`（既定 120 秒）内に完了しない場合、IDP 実行 API が 502（`document-analysis-failed`）ではなく 500（`internal-error`）を返し、失敗実行が `idp_runs` に記録されない。

### 再現手順

1. DI 接続設定済みの環境で `FACTONE_DI_TIMEOUT_SECONDS` を極端に小さく（例: 1）する
2. 文書を登録し `POST /api/v1/documents/{document_id}/idp-runs` を呼ぶ
3. 応答と `GET /api/v1/documents/{document_id}/idp-runs` を確認する

### 期待する動作

- 502 `document-analysis-failed` が返る
- `status=failed`・`error_info` にタイムアウトの旨を持つ実行が履歴に追記される（DI 失敗時の既存挙動と同じ）

### 実際の動作

- 500 `internal-error` が返り、実行ログに何も記録されない
- 原因: `AzureDocumentAnalyzer.analyze`（`api/app/services/di.py`）の `poller.result(timeout=...)` は azure-core の仕様上タイムアウト時に例外を送出せず戻る。未完了の応答本文には `analyzeResult` が無く `deserialized` が `None` になり、後続の属性参照で `AttributeError` になる。`AzureError` ではないため `DiError` に変換されない
- 本ブランチ（#113）以前から同じ経路で発生する（`result.documents` が `None` で `AttributeError`）。#113 の最終レビューで検出

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 環境

ローカル / Azure 共通（DI 接続設定がある環境）。修正案: `poller.wait(timeout)` 後に `poller.done()` を確認し、未完了なら `DiError("Document Intelligence request timed out")` を送出する。あわせてフェイククライアントを差し替えた `AzureDocumentAnalyzer.analyze` の単体テストを追加する。

