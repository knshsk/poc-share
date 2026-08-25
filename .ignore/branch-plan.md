# 実装ブランチの分割計画

`.ignore\brainstorm.md`に基づいて設計実装を進めていくが、人間が実装内容を追えるようにしたい。

そのため、開発初期フェーズから開発方針の「プロジェクト·ソースコード管理」に則ってブランチを分割して実装を進めていく。

このドキュメントでは、そのブランチ分割計画を人間とエージェントで共同編集して決めていく。

---

## 基本方針

- 分割軸: ハイブリッド。序盤=レイヤー軸で基盤群を確定列挙、中盤以降=利用シナリオ単位の垂直スライス
- 記述粒度: ローリングウェーブ。序盤は確定、中盤は候補リスト+方針のみ。着手前に随時詳細化
- 初回コミット: ブートストラップ例外。最小構成(README・.gitignore・CLAUDE.md)のみmainへ直接push。以降すべてPR(手順=S0)
- 1ブランチ=1関心事。レビュー30分以内目安

## 序盤: 基盤ブランチ群(確定)

依存順。仮ID=S1〜S8、Issue起票時に実番号確定。

- [x] **S0 bootstrap** — 初回コミット。ブートストラップ例外としてmainへ直接push(本件のみ。以降PR必須)
  1. 対象ファイル確認: README.md・.gitignore・CLAUDE.mdの3点のみステージ(`.github/`・`docs/`はS1へ回す)
  2. `.gitignore`に`.ignore/`除外が含まれること確認
  3. `git add README.md .gitignore CLAUDE.md`
  4. `git commit -m "chore: bootstrap repository"`
  5. リモート設定確認後`git push origin main`
  6. GitHub側設定(push直後に実施。詳細下記)

### S0-6 GitHub側設定詳細

リポジトリ設定(Settings → General):

- マージ方式: squash mergeのみ許可(merge commit・rebase無効化)。既定コミットメッセージ=PRタイトル
- ブランチ自動削除: 無効化(PoC中はマージ済ブランチを実装履歴として残す)

ルールセット1: main保護(Settings → Rules → Rulesets):

- 対象: default branch(main)
- Enforcement: Active
- Bypass list: なし
- ルール:
  - Require a pull request before merging(直接push禁止はこれで実現)
    - 必要承認数: PoC体制(人間1名+エージェント)のため0。セルフマージ許容、レビューはPRチェックリストで担保
  - Block force pushes
  - Restrict deletions

ルールセット2: ブランチ作成制限:

- 対象: 全ブランチ(`~ALL`)、除外指定=`main`・`feature/*`・`fix/*`
- Enforcement: Active
- ルール: Restrict creations(パターン外ブランチ作成を拒否)

留意:

- GitHub Enterprise前提。Organization側ポリシーとの競合時はOrg設定優先
- ステータスチェック必須化はCI見送りのため設定しない(CI導入時に追加)
- Issue設定はテンプレ側で対応済(config.ymlで空白Issue無効化。S1でコミット)
- `.ignore\branch-plan.md`はリポジトリに含まれないため、IssueおよびPRにファイル名と項番を含めない
- `.ignore\brainstorm.md`もリポジトリに含まれないため、IssueおよびPRに当該ファイル中で割り当てられているIDなどを含めない
- [x] **S1 repo-scaffolding** — .githubテンプレ一式、docs骨格、ディレクトリ雛形。レビュー対象=規約類
- [x] **S2 local-stack** — compose.yaml(Keycloak+PostgreSQL)、keycloak/realm JSON初版、起動手順(develop-guide最小)
- [x] **S3 api-skeleton** — uv+FastAPI起動、healthエンドポイント、pytest器、設定読込骨格(Issue #7 / PR #8。Python 3.14採用)
- [x] **S4 viewer-skeleton** — Vite+Vue+Vuetify+Pinia起動、Vitest器(Issue #9 / PR #10。Node.js 24 LTS採用、アイコンライブラリ未導入)
- [x] **S5 quality-gate** — pre-commit(ruff・mypy・eslint・prettier)、品質コマンド入口一元化。S3・S4後(lint対象必要)(Issue #11 / PR #12。mypy strict、テストはpre-commit対象外)
- [x] **S6 api-contract** — openapi-typescript生成パイプライン+pre-commit差分チェック。S5後(Issue #13 / PR #14。留意: openapi-typescriptのpeer宣言がTS 5系のままでoverrides暫定対応中。上流追従で解除、長期停滞時は生成専用package.jsonへ隔離)
- [x] **S7 bicep-core** — 最小bicep(DI・Blob・PostgreSQL・Key Vault)+デプロイスクリプト初版。他と独立・並行可(Issue #15 / PR #16。実デプロイ未実施=az bicep buildまで。環境名4文字制限)
- [x] **S8 auth-integration** — SPA+OIDC/PKCE、FastAPI JWT検証、realm定義拡充。S2〜S4後(Issue #17 / PR #18。ADR-0001作成。留意: Keycloakのrealmインポートは環境変数置換非対応→テストユーザーパスワードはkcadmスクリプトで起動後設定)

S8完了=「認証付き骨格が動く」状態。垂直スライスへ移行。

## 中盤: 垂直スライス候補(順序仮)

着手前に詳細化必須。ブロッカー(Q-IDPV)解消前の着手禁止(仮置き明記を除く)。

- [x] **doc-ingest** — 文書メタデータモデル+Blob保存+文書登録API(COM-01/02基盤)。垂直スライス起点(Issue #19 / PR #20。仮置き: tenant_id固定値・単一テーブル+種別列。M2Mクライアントはfactone-integration命名)
- [x] **idp-api** — DIアダプタ(prebuilt-layout+Query Fields)+IF-01/02+実行ログ(IDP-01〜05)(Issue #37 / PR #38。仮置き: Q-IDPV-01=クエリセットはサンプルPDF様式暫定、Q-IDPV-10=同期応答・タイムアウト120秒既定。WBS対象外。run_typeサーバ自動判定。実DI疎通確認済。付随: アプリ設定の.env参照をルート固定に集約=cwd非依存)
- [x] **viewer-display** — PDF→PNG変換+SVGオーバーレイ+APIプロキシ配信(VWR-01)(Issue #21 / PR #22。ページ画像URLは/pages/{page}/image=拡張子なし。関連fix: Vuetify未登録バグ Issue #23 / PR #24)
- [x] **view-url** — 閲覧URLトークン発行・検証・失効+DB管理(VWR-02)。Q-IDPV-07は仮置き進行可(Issue #25 / PR #26。ADR-0002=共有可否の型属性宣言。トークンはSHA-256ハッシュ保存。レート制限はインメモリ=スケールアウト時要外部化)
- [x] **view-events** — 閲覧・DLイベント記録(VWR-03)。viewer-display・view-url後(Issue #27 / PR #28。冪等キーはUNIQUE制約で担保。社内閲覧は非記録=仮置き。外部連携IF-04はevent-exportで)
- [ ] **internal-verify** — 注文書社内照合画面(VWR-04)。ブロッカー: Q-IDPV-05
- [x] **idp-rerun** — Viewer画面からIDP再実行+結果表示(VWR-05・IF-05)(Issue #39 / PR #40。Viewer側のみ追加=IdpRunPanel.vue新規+DocumentView組込。API変更なし。矩形オーバーレイなし=実行応答に座標情報なし。再実行操作・結果表示は社内のみ=PublicView変更なし)
- [ ] **event-export** — 閲覧ログ外部連携(IF-04)。ブロッカー: Q-IDPV-03
- [x] **deploy-infra** — Container Apps・ACRのbicep+Containerfile(マルチステージ)+az acr buildスクリプト。S3・S4後なら着手可(Issue #31 / PR #32。実デプロイ検証済=ログインまで確認。ユーザー割当ID採用・KC_HOSTNAME必須・revisionSuffixで再プル強制。Keycloakシークレット/redirect_uriはkcadm手動=スクリプト化候補)
- [x] **samples** — 検証用サンプルPDF+作成規約(develop-guide)(Issue #29 / PR #30。スクリプト生成統一・IPAexフォント自動取得。様式は仮置き=確定項目待ち)

## 運用ルール

- 命名: `feature/<issue番号>-<説明>`・`fix/<issue番号>`。計画上は仮IDで管理、起票時に実番号確定
- フロー: 候補詳細化 → Issue起票(テンプレ使用)→ ブランチ作成 → 実装 → PR(チェックリスト充足)→ レビュー → マージ
- マージ方式: squash merge推奨。1PR=1コミットでmain履歴と計画が1:1対応
- 完了条件(全ブランチ共通): 品質ゲート通過。「厚くする」領域該当時テスト添付。ADR対象決定含む場合ADR起票
- 計画の保守: 本ファイルを唯一の正とする。着手時詳細化、完了時チェック。乖離時は実態に合わせ更新
