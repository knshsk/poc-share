title:	[Feature] Viewer 読取結果の修正機能と DI 解析結果の保存・取得 API 再編（親Issue）
state:	CLOSED
labels:	feature
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	
sub-issues:	shiro-ino/factone-idp-viewer#113, shiro-ino/factone-idp-viewer#114, shiro-ino/factone-idp-viewer#115, shiro-ino/factone-idp-viewer#116, shiro-ino/factone-idp-viewer#117, shiro-ino/factone-idp-viewer#118
sub-issues-completed:	6/6
blocked-by:	
blocking:	
number:	112
--
### 目的

SAP 承認時の原本照合で、Viewer 上で IDP の読取結果を確認・修正し、修正結果を外部（WalkMe / Power Automate）が API で取得できるようにする。あわせて Document Intelligence の生応答を将来の機能拡張・カスタムモデル学習に備えて保存し、解析結果 API を応答サイズの観点で再編する。

### 変更内容

設計は docs/superpowers/specs/2026-09-13-viewer-result-editing-design.md（ローカル管理）に基づく。Sub-issue で分割実施する。

1. #113 API: DI 生応答の Blob 保存と実行時情報（API バージョン・モデル ID・読取領域）の記録
2. #114 API: 解析結果 API の一覧サマリ化・詳細・latest 分割（外部利用者未接続のため v1 内で実施）— 依存: #113
3. #115 API: 人手修正の保存（`idp_runs` に `corrected` 行を追記、修正 API）— 依存: #114
4. #116 Viewer: キャンバスのフィットズーム 3 種・初期全体表示・フッター・右ペイン幅 528px — 並行可
5. #117 Viewer: 右ペインの 3 区分表示（キー項目 / キー項目候補 / グリッド）とテキストボックス編集・確定 UI — 依存: #114, #115
6. #118 Viewer: キャンバス（矩形）⇄ 右ペイン（入力欄）の相互ハイライト — 依存: #113, #117

UI は Vuetify 製モックアップ（`.ignore/mockup/`、リポジトリ管理対象外）で確認済み。

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- 全 sub-issue の PR がマージされている
- ADR-0018（人手修正の追記保存）・ADR-0019（生応答の Blob 保存）が作成され、ADR-0017 に v1 内破壊的変更の記録が追記されている
- `docs/data-model/idp-runs.md`・`docs/api/README.md`・`docs/viewer/`・`docs/requirements/viewer.md`（VW-09）が更新されている

### 関連情報

- ADR-0005（追記型・生値と正規化値の分離）、ADR-0010（Blob の API プロキシ）、ADR-0017（API バージョニング）
- #107（正規表現パターン抽出）

