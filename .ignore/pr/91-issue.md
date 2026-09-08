title:	[Feature] 文書種別fieldsにViewer表示可否フラグ（visible）を追加
state:	CLOSED

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
number:	91
--
### 目的

Query Fieldsで意味合いによる値を取得する際、送信先／送信元のように対になっている項目は両方指定した方が精度が上がる。一方で、受領した注文書に記載される自社名など、Viewerに結果を表示する必要がない項目も存在する。エンドユーザーの利便性向上のため、フィールドごとにViewerで可視化するかを設定できるようにする。

### 変更内容

- `document_type_configs.fields[]` に `visible: bool`（デフォルト true）を追加する。既存行は未指定 = true として扱い、データ移行は行わない
- IDP実行（Query Fields・抽出・保存・API返却）は非表示項目も従来どおり扱う。非表示はViewer表示のみ
- Viewerは取得済みの文書種別設定から `visible=false` のキーを除外して読み取り結果を表示する（表示時に現在設定を参照。過去実行分にも反映）
- 設定取得失敗・設定なしの場合は全項目表示（既存フォールバック方針を踏襲）
- `docs/api/openapi.json`・`viewer/src/api/schema.d.ts` を再生成し、`docs/data-model/document-type-configs.md` を更新する

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] 設定CRUD APIで `visible` を省略時 true・指定値で往復保持できる
- [ ] Viewerで `visible=false` の項目が読み取り結果に表示されない
- [ ] `visible` 未指定・設定取得失敗時は全項目表示される
- [ ] API・Viewerのテストを追加し品質ゲートを通過する

### 関連情報

- ADR-0014 文書種別のテナント別設定値化
- #83 読み取り結果の項目名を実行時ラベルで表示
