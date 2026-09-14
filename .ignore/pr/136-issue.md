title:	[Feature] DI読取値の正規化規則を緩和し、Viewerの入力欄初期値を正規化値にする
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
number:	136
--
### 目的

number / date 型の正規化判定が厳しく（number はカンマ除去した整数のみ、date は `YYYY-MM-DD` / `YYYY/MM/DD` のみ）、実務書類で DI が読む `￥74,800` や `令和 8年 9月 4日` が「解釈できません」警告になる。人手転記の省力化という目的に沿い、初めから `74800` / `2026-09-04` が入力欄に入っている状態にする。

### 変更内容

- サーバ `_normalize`（`api/app/services/idp_extraction.py`）の規則緩和
  - number: NFKC 正規化 → `¥ 円 , 、 空白` 除去 → 整数化
  - date: NFKC 正規化 → 西暦 `YYYY[-/.年]M[-/.月]D[日]`（ゼロ埋め任意）と和暦 `(令和|平成|昭和|R|H|S)N年M月D日`（元年・空白許容）を ISO 8601 に変換
- クライアント `viewer/src/utils/normalize.ts` を同規則に更新
- `ResultEditor` のキー項目入力欄の初期値を `normalized_fields`（null なら `raw_fields`）にし、原典（`raw_fields`）が異なるときは「読取値 …」を補足表示する
- 関連文書（`docs/data-model/idp-runs.md`、`docs/viewer/result-panel.md`）更新

### 対象コンポーネント（複数選択可）

API（`api` 配下）, 画面（`viewer` 配下）, ドキュメント（`docs` 配下）

### 完了条件

- [ ] `￥74,800` → 74800、`令和 8年 9月 4日` → `2026-09-04` に正規化される（サーバ・クライアント両方のテスト）
- [ ] 入力欄の初期値が正規化値になり、原典が異なる場合は読取値が補足表示される
- [ ] 正規化不能な値は従来どおり生値 + 警告表示
- [ ] 文書更新

### 関連情報

ADR-0005（生値/正規化値の分離。方針変更なし）、#112、#128
