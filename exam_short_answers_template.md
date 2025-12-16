# Associate Cloud Engineer 試験 分野別・短問回答テンプレート

目的: 想定問→最適解→代替案→落とし穴の4点で、素早く回答を組み立てるための雛形。

---

## 使い方
- 各分野のテンプレート内の角括弧 [] を具体値で置換。
- 実務前提の前提条件/制約（リージョン、SLA、セキュリティ要件、コスト条件）を先頭に明記。
- 可能であれば `page-XXX.txt` など出典をリンク。

---

## 1. 設計・計画 (Planning)

- 想定問: [例: 単一ゾーン運用の可用性を高めたい。最小コストで?]
- 最適解: [MIG を複数ゾーンで構成し、HTTP(S) LB と Autohealing を併用]
- 代替案: [Regional 内の冗長化のみ/Spot VM 併用/Run に寄せる 等]
- 落とし穴: [単一ゾーン依存のまま LB だけ追加/ヘルスチェック未設定/ステートフル要件未確認]

### サンプル短問
- 想定問: バックアップからの復元時間を短縮しつつ RPO を最小化したい
	- 最適解: Cloud SQL の自動バックアップ + PITR を有効化
	- 代替案: Export/Import + スタンバイレプリカ
	- 落とし穴: バックアップ検証未実施/メンテナンスウィンドウ未設定
- 想定問: 低遅延グローバル書込が必要
	- 最適解: Cloud Spanner (強整合/水平スケール)
	- 代替案: 地理レプリケーション付き RDB (要整合性要件緩和)
	- 落とし穴: Spanner を RDB の単純代替と誤認/スキーマ設計不備

---

## 2. 導入・実装 (Deploying & Implementing)

- 想定問: [例: Cloud Storage の新規オブジェクトを自動処理したい]
- 最適解: [Cloud Functions もしくは Cloud Run (イベントトリガ) + Pub/Sub バッファ]
- 代替案: [Cron + バッチ定期走行/ワークフロー Orchestrator]
- 落とし穴: [同期処理でタイムアウト/権限をプロジェクト全体に付与/再試行設定なし]

### サンプル短問
- 想定問: IaC で GKE ノードタイプを切り替えたい
	- 最適解: Deployment Manager/Terraform で NodePool を宣言的に管理
	- 代替案: 手動変更 + ドレイン/スケール
	- 落とし穴: 既存 Pod の要求リソース不一致/無停止切替の手順漏れ
- 想定問: CI から Storage と Pub/Sub に安全にアクセス
	- 最適解: Cloud Build サービスアカウントに最小権限付与 (Storage Object Creator/Publisher)
	- 代替案: Workload Identity 連携
	- 落とし穴: エディタ権限付与/鍵共有

---

## 3. 運用の成功保証 (Ensuring Successful Operation)

- 想定問: [例: CPU 90% 超過で即時通知したい]
- 最適解: [Cloud Monitoring アラートポリシー + 通知チャネル (Email/PagerDuty/Slack)]
- 代替案: [GKE は HPA + 監視; MIG は Autoscaler]
- 落とし穴: [手動監視のみ/メトリクス種別誤り/通知チャネル未設定]

- 想定問: [例: 監査ログを分析に回したい]
- 最適解: [Cloud Logging Sink → BigQuery (もしくは Storage) にエクスポート]
- 代替案: [Log Router で Pub/Sub 経由の処理]
- 落とし穴: [Data Access ログを有効化していない/フィルタ過不足]

### サンプル短問
- 想定問: VM の脆弱性を可視化したい
	- 最適解: OS Config/OS Inventory を有効化し、レポートを Monitoring/Asset で閲覧
	- 代替案: サードパーティエージェント導入
	- 落とし穴: メタデータ/権限不足で収集失敗
- 想定問: スパイク時に安定稼働
	- 最適解: MIG Autoscaler/HPA 設定 + キューイングに Pub/Sub
	- 代替案: 事前ウォームアップ/キャッシュ
	- 落とし穴: 同期で詰まり/バックエンド未スケール

---

## 4. アクセスとセキュリティ (Access & Security)

- 想定問: [例: サービスアカウントキーを安全に運用し短期利用に限定したい]
- 最適解: [Secret Manager + 短期クレデンシャル/組織ポリシー/最小権限の個別 SA]
- 代替案: [鍵ハブプロジェクトで集中管理/Workload Identity Federation]
- 落とし穴: [キー共有/長期有効化/プロジェクト全体ロール付与]

- 想定問: [例: VM に外部 IP を持たせずに Google API にアクセス]
- 最適解: [Private Google Access + Cloud NAT (もしくは VPN/Interconnect)]
- 代替案: [プロキシ経由/サイドカー]
- 落とし穴: [外部IP付与/PGAccess未有効/Firewall 誤設定]

### サンプル短問
- 想定問: プロジェクト間でカスタムロールを使い回したい
	- 最適解: 組織レベルのカスタムロール定義 + 継承活用
	- 代替案: 同一ロールを各プロジェクトで複製
	- 落とし穴: 権限差異/更新の同期漏れ
- 想定問: 秘密情報の配布を安全にしたい
	- 最適解: Secret Manager + Accessor 付与 + バージョニング/ローテーション
	- 代替案: KMS で暗号化 + Storage 保管
	- 落とし穴: 平文保管/共有鍵/長期有効

---

## 5. 環境セットアップ (Environment Setup)

- 想定問: [例: 請求を中央管理へ変更したい]
- 最適解: [請求アカウントを組織に紐付け、プロジェクトを集中管理。フォルダ構造と IAM を整理]
- 代替案: [ラベルで請求集計/BigQuery 請求エクスポートの分析]
- 落とし穴: [個別請求のまま/オーナー権限乱用/権限継承を過剰に許可]

### サンプル短問
- 想定問: 新規プロジェクトの初期設定
	- 最適解: API 有効化/サービスアカウント/ロール付与/監視・ログの基本設定をテンプレ適用
	- 代替案: ベースラインをスクリプトで適用
	- 落とし穴: 権限過剰/監査ログ未確認

---

## 6. データストア選定 (SQL/Spanner/Bigtable/BigQuery)

- 想定問: [例: 時系列で低レイテンシ大量書込]
- 最適解: [Cloud Bigtable]
- 代替案: [Pub/Sub バッファ + Cloud Storage 原始保管 + 後段集計]
- 落とし穴: [RDB を水平スケールで代替しようとする/行キー設計不備]

- 想定問: [例: 既存 PostgreSQL を変更最小で移行]
- 最適解: [Cloud SQL for PostgreSQL + HA + PITR]
- 代替案: [Spanner (要スキーマ再設計)/Managed 以外]
- 落とし穴: [バックアップ無/接続方式・認証未整理]

- 想定問: [例: 異種データを移動せず分析]
- 最適解: [BigQuery External Table/Federated Query]
- 代替案: [Storage へ集約してからロード]
- 落とし穴: [スキーマ/権限整備不足/コスト見積り不備]

### サンプル短問
- 想定問: 誤削除に備えたリストア
	- 最適解: Cloud SQL PITR/自動バックアップ
	- 代替案: Export/Import + バージョニング
	- 落とし穴: 退避先の権限不足/復元手順未検証
- 想定問: 大量イベントの着地設計
	- 最適解: Pub/Sub → Dataflow → BigQuery/Bigtable
	- 代替案: Storage 経由のバッチロード
	- 落とし穴: スキーマ進化未考慮/遅延許容未定義

---

## 7. ロードバランシング/可用性

- 想定問: [例: ゾーン障害でも継続運用]
- 最適解: [Regional MIG (複数ゾーン) + HTTP(S) LB + Autohealing]
- 代替案: [Global LB + 複数バックエンド/Run/GKE]
- 落とし穴: [単ゾーン/MIG 未利用/ヘルスチェック不備]

### サンプル短問
- 想定問: 外部公開と WAF を組み合わせたい
	- 最適解: HTTP(S) LB + Cloud Armor ポリシー
	- 代替案: アプリ側レート制限/別製品 WAF
	- 落とし穴: L4 LB 選択/ポリシー適用範囲誤り
- 想定問: 静的配信の最適化
	- 最適解: Cloud Storage + HTTP(S) LB + Cloud CDN
	- 代替案: Run から直接配信
	- 落とし穴: キャッシュ制御ヘッダ不備/オリジン認証未設定

---

## 8. 参考リンク (社内用に差し替え)
- [page-072.txt](page/page-072.txt): ゾーン障害対策のケース (MIG + LB)
- [page-060.txt](page/page-060.txt): Cloud SQL の PITR/バックアップ
- [page-068.txt](page/page-068.txt): 異種ストア横断分析 (Spanner + Bigtable → BigQuery)
- [page-124.txt](page/page-124.txt): Cloud Build の認証と他サービス連携
- [page-100.txt](page/page-100.txt): バースト処理に Pub/Sub を挟むアーキテクチャ
- [page-172.txt](page/page-172.txt): Dataflow によるリアルタイム収集の耐障害構成

---

テンプレは必要に応じて分野を追加してください。