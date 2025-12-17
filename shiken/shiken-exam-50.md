# Professional Cloud Developer 模擬試験 50問（解説付き）

最終更新: 2025-12-17
出題根拠: `shiken-translation.md`（試験ガイド）に準拠

配分目安（試験ガイド比率）
- セクション1（設計）: 18問
- セクション2（構築/テスト）: 12問
- セクション3（デプロイ）: 10問
- セクション4（統合/運用）: 10問

---

## セクション1: 設計（18問）

Q1. API のレイテンシを最小化したい。リージョン/ゾーンの選定で最も重要な考慮はどれか。
- 解答: ユーザ/依存サービスとの地理的近接性に基づくリージョン選定（レイテンシ/可用性/法規制）
- 解説: 試験ガイド「サービスの地理的分布（レイテンシ、リージョナル/ゾーナル）」。

Q2. セッションアフィニティが必要な Web アプリで推奨のロードバランサ構成は？
- 解答: HTTP(S) LB のバックエンドでセッションアフィニティ（クライアントIP/ヘッダ）設定。
- 解説: 「ロードバランサとアプリのセッションアフィニティ」。

Q3. キャッシュ導入の代表選択肢は？
- 解答: Memorystore (Redis)
- 解説: 「キャッシュソリューション（Memorystore）」。

Q4. gRPC API の利点は？
- 解答: 双方向ストリーミング・型安全・高速通信。
- 解説: 「HTTP REST/gRPC」。

Q5. レート制限と認証を統合的に管理する最適なサービスは？
- 解答: Apigee（もしくは API Gateway）
- 解説: 「レート制限・認証・可観測性（Apigee, API Gateway）」。

Q6. 非同期/イベント駆動統合の推奨は？
- 解答: Eventarc + Pub/Sub を用いた疎結合連携。
- 解説: 「非同期/イベント駆動（Eventarc, Pub/Sub）」。

Q7. コスト最適化の基本戦略は？
- 解答: オートスケール/並行数/最小インスタンス調整、キャッシュ、適切なデータ層選択。
- 解説: 「コスト/リソース最適化」。

Q8. リージョン障害に耐えるデータ設計指針は？
- 解答: リージョン/ゾーン冗長のレプリケーション（Spanner, Cloud Storage マルチリージョン等）。
- 解説: 「フェイルオーバモデルとデータレプリケーション」。

Q9. 段階的リリースの安全策は？
- 解答: トラフィック分割（Cloud Run/GKE リビジョン）とロールバック計画。
- 解説: 「Traffic splitting strategies」。

Q10. Workflows/Cloud Tasks/Cloud Schedulerの使い分けは？
- 解答: Workflows=オーケストレーション、Tasks=非同期タスク制御、Scheduler=定時トリガ。
- 解説: 「サービスオーケストレーション」。

Q11. IAP の主目的は？
- 解答: アイデンティティを用いたアプリ/HTTP リソースへのアクセス制御（ゼロトラスト）。
- 解説: 「脆弱性保護: IAP」。

Q12. Artifact Analysis 検出の脆弱性対応は？
- 解答: 修正→再ビルド→署名/プロビナンス付与→デプロイ前にポリシー検証。
- 解説: 「Artifact Analysis と SCC」。

Q13. シークレット管理の推奨は？
- 解答: Secret Manager＋最小権限SA＋ローテーション自動化。
- 解説: 「Secret Manager, KMS, WIF」。

Q14. ADC とサービスアカウント鍵の比較で推奨は？
- 解答: 可能なら鍵配布を避け ADC/WIF を利用。
- 解説: 「Authenticating to Google Cloud」。

Q15. サービス間通信の保護に適切なのは？
- 解答: Cloud Service Mesh または Kubernetes NetworkPolicy。
- 解説: 「Securing service-to-service communications」。

Q16. Binary Authorization の役割は？
- 解答: 署名/ポリシーに基づきデプロイ許可を制御。
- 解説: 「Securing application artifacts using Binary Authorization」。

Q17. Signed URL の用途は？
- 解答: 一時的/限定的に Cloud Storage オブジェクトへのアクセス付与。
- 解説: 「Creating signed URLs」。

Q18. BigQuery への書き込みの代表用途は？
- 解答: 分析/AI/ML ワークロードのデータ蓄積。
- 解説: 「Writing data to BigQuery」。

---

## セクション2: 構築/テスト（12問）

Q19. ローカルで GCP を模擬してユニットテストを行う手段は？
- 解答: Cloud SDK / ローカルエミュレータの利用。
- 解説: 「Emulating Google Cloud services using the Google Cloud CLI」。

Q20. IDE 連携と補助ツールの代表は？
- 解答: Cloud Code / Gemini Code Assist / Cloud Workstations。
- 解説: 「開発ツール群」。

Q21. コンテナのビルド/保管の標準は？
- 解答: Cloud Build + Artifact Registry。
- 解説: 「Using Cloud Build and Artifact Registry」。

Q22. プロビナンス設定の目的は？
- 解答: ビルドの来歴/信頼性を証跡化し、Binary Authorization と連携。
- 解説: 「Configuring provenance in Cloud Build」。

Q23. ユニットテスト支援に関連する生成AIツールは？
- 解答: Gemini Code Assist。
- 解説: 「Writing unit tests with Gemini Code Assist」。

Q24. 統合テストの自動実行はどこで？
- 解答: Cloud Build のパイプライン。
- 解説: 「Executing automated integration tests in Cloud Build」。

Q25. アーティファクト検証をパイプラインに組み込むには？
- 解答: Cloud Build のステップで署名/ポリシー検証を追加。
- 解説: 2.2/1.2 の関連。

Q26. ローカル→クラウドへの一貫した環境提供は？
- 解答: Cloud Workstations。
- 解説: 「Cloud Workstations」。

Q27. CIのキャッシュでビルド高速化するには？
- 解答: Cloud Build のキャッシュ/レイヤ再利用、Artifact Registry のベースイメージ活用。
- 解説: 実務補足（ガイドの趣旨に合致）。

Q28. セキュアな依存管理の要点は？
- 解答: 署名済みアーティファクト・SBOM・脆弱性スキャン。
- 解説: 1.2/2.2 と整合。

Q29. テストと可観測性の接続は？
- 解答: 構造化ログ/トレース埋め込みで失敗時に原因追跡。
- 解説: 4.3 と整合。

Q30. マイクロサービス間契約テストの推奨は？
- 解答: API スキーマ（OpenAPI/gRPC プロト）を基盤に自動検証。
- 解説: 1.1 と整合。

---

## セクション3: デプロイ（10問）

Q31. Cloud Run へのデプロイ元で最も簡便なのは？
- 解答: ソースからのビルド/デプロイ（Cloud Build 統合）。
- 解説: 「Deploying applications from source code」。

Q32. Cloud Run をイベント駆動で起動するには？
- 解答: Eventarc または Pub/Sub のトリガ設定。
- 解説: 「Invoking Cloud Run services using triggers」。

Q33. Eventarc の受信設定の要点は？
- 解答: イベントフィルタ・リージョン・ターゲット（Run/GKE/Functions）。
- 解説: 「Configuring event receivers」。

Q34. API 公開と保護を行うには？
- 解答: Apigee のプロキシ/ポリシーで管理。
- 解説: 「Exposing and securing APIs」。

Q35. Cloud Endpoints の後方互換性を保つには？
- 解答: バージョニングと互換ポリシー、ステージングで検証。
- 解説: 「Deploying a new API version」。

Q36. GKE におけるリソース要件定義は何に影響するか？
- 解答: スケジューリング/安定性/コスト（requests/limits）。
- 解説: 「Defining resource requirements」。

Q37. 可用性向上のための Kubernetes ヘルスチェックは？
- 解答: liveness/readiness/startup probes。
- 解説: 「Implementing Kubernetes health checks」。

Q38. コスト最適化のための HPA 設定例は？
- 解答: CPU/カスタムメトリクスに基づくスケーリングし閾値調整。
- 解説: 「Configuring HPA for cost optimization」。

Q39. Cloud Run でコールドスタートを緩和する設定は？
- 解答: 最小インスタンス数の設定と並行数調整。
- 解説: 1.1 のトラフィック戦略と整合。

Q40. デプロイの段階的リリースを行うには？
- 解答: Cloud Run/GKE のリビジョン/トラフィック分割機能を活用。
- 解説: 1.1 と整合。

---

## セクション4: 統合/運用（10問）

Q41. Cloud SQL/Firestore/Cloud Storage 接続の共通課題は？
- 解答: 認証/接続管理/再試行/バックオフ/接続プーリング。
- 解説: 「Managing connections to datastores」。

Q42. Pub/Sub の基本は？
- 解答: 少なくとも一度配送/冪等設計/Retry/DLT/Ordering Key。
- 解説: 「Publish and consume using Pub/Sub」。

Q43. API 呼び出し時に考慮すべき点は？
- 解答: バッチ、返却制限、ページング、キャッシュ、エラー処理（指数バックオフ）。
- 解説: 「Making API calls...」。

Q44. サービスアカウントで API を呼ぶ利点は？
- 解答: 最小権限の機械アイデンティティで安全に自動化。
- 解説: 「Using service accounts to make Cloud API calls」。

Q45. 可観測性の三本柱は？
- 解答: メトリクス・ログ・トレース。
- 解説: 「Instrumenting code to facilitate troubleshooting」。

Q46. 問題特定の第一歩は？
- 解答: Cloud Observability のダッシュボード/ログクエリ/トレースから症状把握。
- 解説: 「Identifying and resolving issues」。

Q47. Error Reporting の役割は？
- 解答: 例外の集約/重複排除/通知/トレンド把握。
- 解説: 「Managing application issues using Error Reporting」。

Q48. Trace ID の活用は？
- 解答: サービス横断のスパン相関による経路追跡。
- 解説: 「Using trace IDs to correlate」。

Q49. Gemini Cloud Assist の適用場面は？
- 解答: 設定/運用の支援、トラブル時のガイド、改善提案。
- 解説: 「Using Gemini Cloud Assist」。

Q50. 運用改善の継続的アプローチは？
- 解答: SLO/SLI 設計→アラート→ポストモーテム→改善サイクル。
- 解説: 試験ガイドの観測/運用の趣旨に合致。

---

注記:
- 各問は試験ガイドの要点に基づく実践的な設問です。実際の試験はケーススタディ/選択肢形式です。詳しい設定は公式ドキュメントをご参照ください。
