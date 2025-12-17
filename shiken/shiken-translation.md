# Professional Cloud Developer 試験ガイド（日本語訳）

最終更新: 2025-12-17

この文書は `image-0.png`〜`image-3.png` の英語原文（OCR）に基づき、日本語で要点を整理したものです。

---

## 概要

Professional Cloud Developer は、Google 推奨のツールやベストプラクティスを用いて、スケーラブル・セキュア・高可用なアプリケーションを構築・デプロイします。クラウドネイティブ、Google Cloud API、開発者/AI ツール、マネージドサービス、オーケストレーション、サーバレス、コンテナ、テスト/デプロイ戦略、問題の切り分け、データストアに関する経験を有します。少なくとも 1 つの汎用プログラミング言語に習熟し、コードにメトリクス・ログ・トレースを計測可能です。

---

## セクション1: 高スケーラビリティ/高可用/高信頼なクラウドネイティブアプリの設計（試験の約36%）

### 1.1 高性能なアプリケーションと API の設計
- ユースケース/要件に基づく適切なプラットフォーム選定（例: Compute Engine, GKE, Cloud Run）
- コンテナの構築・リファクタリング・デプロイ（Cloud Run / GKE）
- Google Cloud サービスの地理的分布の理解（レイテンシ、リージョナル/ゾーナル）
- ロードバランサ/アプリのセッションアフィニティ設定と高速なコンテンツ配信
- キャッシュの導入（例: Memorystore）
- API の作成/展開（HTTP REST, gRPC）
- レート制限・認証・可観測性（Apigee, API Gateway）
- 非同期/イベント駆動の統合（Eventarc, Pub/Sub）
- コスト/リソース最適化
- ゾーン/リージョンのフェイルオーバに対応したデータレプリケーションの理解
- トラフィック分割（段階的ロールアウト、ロールバック、A/B テスト）
- Workflows, Eventarc, Cloud Tasks, Cloud Scheduler によるサービスオーケストレーション

### 1.2 セキュアなアプリケーションの設計
- データ保持/編成ポリシー（Cloud Storage ライフサイクル、保持/ロック）
- 脆弱性の検出/保護（IAP, Web Security Scanner）
- Artifact Analysis / Security Command Center による脆弱性対応
- シークレット/資格情報/鍵の保管・アクセス・ローテーション（Secret Manager, Cloud KMS, Workload Identity Federation）
- Google Cloud への認証（ADC, JWT, OAuth 2.0, Cloud SQL/Auth Proxy, AlloyDB/Auth Proxy）
- エンドユーザ認証管理（Identity Platform）
- IAM ロールによるサービスアカウントの権限管理
- サービス間通信の保護（Service Mesh, Kubernetes NetworkPolicy）
- 最小権限でのサービス実行
- バイナリ認証（Binary Authorization）でアーティファクトを保護

### 1.3 データの保存とアクセス
- データ量/性能要件に基づく適切なストレージ選定
- 構造化 DB（AlloyDB, Spanner）/非構造化 DB（Bigtable, Datastore）スキーマ設計
- 各サービスの整合性モデルの把握（AlloyDB, Bigtable, Cloud SQL, Spanner, Cloud Storage）
- 署名付き URL（Signed URL）で Cloud Storage オブジェクトへのアクセス付与
- BigQuery へのデータ書き込み（分析/AI/ML）

---

## セクション2: アプリケーションの構築とテスト（試験の約23%）

### 2.1 開発環境のセットアップ
- ローカル開発/ユニットテストでの GCP サービスのエミュレーション（gcloud CLI）
- コンソール、Cloud SDK、Cloud Code、Gemini Cloud Assist、Gemini Code Assist、Cloud Shell、Cloud Workstations の活用

### 2.2 ビルド
- Cloud Build と Artifact Registry によるコンテナのビルド/保管
- Cloud Build でのプロビナンス設定（Binary Authorization）

### 2.3 テスト
- Gemini Code Assist によるユニットテスト作成支援
- Cloud Build による自動統合テストの実行

---

## セクション3: アプリケーションのデプロイ（試験の約20%）

### 3.1 Cloud Run へのデプロイ
- ソースコードからのデプロイ
- Eventarc / Pub/Sub によるトリガで Cloud Run を起動
- Eventarc / Pub/Sub のイベント受信設定
- Apigee による API 公開/保護
- Cloud Endpoints の新バージョンを後方互換性を考慮して展開

### 3.2 GKE へのコンテナデプロイ
- コンテナ化アプリの展開
- コンテナワークロードのリソース要件定義
- Kubernetes のヘルスチェックで可用性向上
- HPA（Horizontal Pod Autoscaler）でコスト最適化

---

## セクション4: Google Cloud サービスとの統合（試験の約21%）

### 4.1 データ/ストレージサービスとの統合
- Cloud SQL / Firestore / Cloud Storage など各種データストアへの接続管理
- 各データストアへの読み書き
- Pub/Sub を用いたデータの発行/購読

### 4.2 Google Cloud API の利用
- サービスの有効化
- Cloud Client Libraries / REST API / gRPC / API Explorer による API 呼び出し（以下を考慮）
  - バッチリクエスト
  - 返却データの制限
  - ページネーション
  - キャッシュ
  - エラー処理（指数バックオフ）
- サービスアカウントを用いた API 呼び出し

### 4.3 トラブルシューティングと可観測性
- メトリクス/ログ/トレースによる計測（Cloud Observability）
- Cloud Observability を用いた問題の特定/解決
- Error Reporting によるアプリケーション問題管理
- Trace ID によるサービス間トレースの相関
- Gemini Cloud Assist の活用

---

※ この日本語訳は学習補助目的であり、試験範囲の要点を簡潔にまとめています。詳細は公式ドキュメントを参照してください。
