# サーバレス / イベント駆動 まとめ

最終更新: 2025-12-17

サーバレスとイベント駆動は、可用性・スケーラビリティ・運用効率を高いレベルで両立するための中心的アーキテクチャ。GCPでは Cloud Functions / Cloud Run / Eventarc / Pub/Sub / Workflows / Cloud Scheduler などを組み合わせ、非同期・疎結合・拡張容易なシステムを構築できる。

## 主要サービスの要点

- Cloud Functions (Gen2)
  - イベント駆動の関数実行。Cloud Storage・Pub/Sub・Firestore・Eventarc などのトリガーに反応。
  - 実行時間・メモリ制限あり。短時間の処理やファンアウト起点・軽量ETLに適合。
  - `roles/functions.invoker` でHTTP保護、Serverless VPC Access Connectorでプライベートリソースへ到達。
  - ベストプラクティス: 冪等処理、再試行対応、入力検証、秘密情報は Secret Manager。

- Cloud Run (サービス/ジョブ)
  - 任意コンテナのサーバレス実行。HTTPサービスは自動スケール/高い並行処理が可能、ジョブはバッチ/非HTTP。
  - リビジョン・トラフィック分割・最小インスタンス設定でコールドスタート緩和と安全なリリース。
  - `roles/run.invoker` で呼び出し制御、VPC Connector/PSConnectで内外通信制御。
  - 適用領域: API・Webhook・画像/動画処理・長めのワーカー・Pub/Sub サブスクライバ。

- Eventarc
  - さまざまなイベントソース (Audit Logs/Cloud Storage/Workflows ほか) を Cloud Run/GKE/Functions にルーティング。
  - フィルタやリージョン指定でターゲット最適化。内部的にPub/Subを活用。
  - 監査ログ起点の自動処理・運用自動化に強い。

- Cloud Pub/Sub
  - 大規模分散メッセージング。Push/Pull、Ordering Key、Dead Letter、Retry、Filter。
  - デリバリは少なくとも一度。必ず冪等処理にすること。最大メッセージサイズは10MB。
  - ロール: `roles/pubsub.publisher` / `roles/pubsub.subscriber`。

- Workflows
  - サーバレスのオーケストレーション。HTTP呼び出し・分岐・リトライ・エラー処理を宣言的に記述。
  - API連携や多段ETL、長時間のビジネスプロセス制御に適合。

- Cloud Scheduler
  - 予備トリガー。HTTP/Cloud Run/Functions/PubSub を定期的に起動。
  - バッチ起動や再送キック、夜間集計ジョブなどに利用。

## 設計指針 / パターン

- 疎結合・非同期: Producer→Pub/Sub→Consumer で独立進化可能に。
- 冪等性: 再試行・重複配送に耐えるよう、自然キー/去重テーブル/条件付き書き込みを活用。
- イベントフィルタリング: Pub/Sub Filter / Eventarc 条件で不要イベントを抑制。
- ファンアウト/ファンイン: 一イベントから複数処理へ分岐、集約はキュー/DB/Workflowsで整合。
- エラー処理: Retry/Backoff、Dead Letter Topic、Poison メッセージ監視。
- 性能/コスト: Cloud Run の並行数・最小インスタンスでレイテンシと料金のバランス調整。
- ネットワーク/セキュリティ: Serverless VPC Access でプライベート到達、最小権限のサービスアカウント/IAM。

## 観測/運用

- ログ: Cloud Logging に出力・構造化ログで検索/相関容易に。
- メトリクス/アラート: Cloud Monitoring でエラー率/遅延/サブスクバイト/未処理数を監視。
- トレース: Cloud Trace/Profiler を使いボトルネック解消。
- デプロイ: Cloud Build/Artifact Registry と合わせてリビジョン/ロールアウト管理。

## 代表的アーキテクチャ例

- 画像サムネイル生成: Cloud Storage→Eventarc/Functions→Cloud Run (画像処理)→保存。
- 監査ログ駆動自動化: Audit Logs→Eventarc→Cloud Run→通知/是正処理。
- ストリーミングETL: Producer→Pub/Sub→Cloud Run サブスクライバ→BigQuery/Bigtable にロード。
- 予約バッチ: Cloud Scheduler→Pub/Sub→Cloud Run Jobs/Workflows→集計/レポート。

## 選定の目安 (Cloud Functions vs Cloud Run)

- Cloud Functions: 短時間・シンプル処理、トリガ駆動、素早い開発。
- Cloud Run: コンテナ自由度、並行処理、高度な依存や長め処理、堅牢なリリース戦略。

## 試験頻出ポイント (Developer/ACE)

- 非同期/疎結合には Pub/Sub。冪等性と DLT は必須。
- 監査ログ起点は Eventarc。HTTP保護は `roles/run.invoker`/`roles/functions.invoker`。
- プライベート到達は Serverless VPC Access。秘密は Secret Manager。
- コールドスタート緩和は Cloud Run の最小インスタンス/トラフィック分割。
- スケジュール実行は Cloud Scheduler→Pub/Sub or HTTP。

*このまとめは学習用に要点を整理したものです。詳しい実装は各サービスのドキュメントを参照してください。*