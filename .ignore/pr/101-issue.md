title:	[Feature] APIパスにバージョン（/api/v1）を導入する
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
number:	101
--
### 目的

将来の破壊的変更を外部呼出元（Power Automate 等）や Viewer に影響なく行えるよう、API に明示的なバージョンを導入する。現時点で外部利用者が存在せず URL 変更コストがゼロであるため、今の時点で方針を確定する。

### 変更内容

- 認証あり API（`/api/**` 15 ルート）を `/api/v1/**` に移す。`/view/{token}/**`（顧客配布 URL）と `/health` は無版のまま
- 版はメジャーのみ、破壊的変更時に限り上げる。追加的変更は同一版内
- 旧 `/api/**` の互換ルート・リダイレクトは作らない（利用者なし）
- OpenAPI・TS 型を再生成し、Viewer の呼出パスを更新する
- 無版の API パスが混入しないことを検証するテストを追加する
- ADR-0017 起票、`docs/api/README.md` にバージョニング方針を追記、関連ドキュメントのパス表記を更新

### 対象コンポーネント（複数選択可）

- API（`api` 配下）
- 画面（`viewer` 配下）
- ドキュメント（`docs` 配下）

### 完了条件

- [ ] `/api/**` の全ルートが `/api/v1/**` に移り、`/view/**` `/health` は変更なし
- [ ] `openapi.json` の全パスが `/api/v1/`・`/view/`・`/health` のいずれかで始まる（テストで検証）
- [ ] Viewer が `/api/v1/**` を呼び、`npm run build` `npm run test` が通過
- [ ] ADR-0017 と `docs/api/README.md` のバージョニング節を追加
- [ ] pre-commit（API 契約ドリフト検出含む）通過

### 関連情報

- ADR-0011（閲覧 URL は不透明トークン。`/view` を無版とする根拠）
- ADR-0016（Problem Details。外部契約を OpenAPI に固定する流れの延長）
