# Google Kubernetes Engine（GKE）まとめ

## 概要
- 対象: マネージド Kubernetes（コントロールプレーン管理不要）
- 主用途: マイクロサービス、CI/CD、ポータビリティ重視のコンテナ運用、オートスケール、ハイブリッド/マルチクラウド

## 利点
- 運用省力: コントロールプレーン管理・アップグレード・パッチ適用をマネージド化（Standard/Autopilot）
- スケーリング: HPA/VPA/Cluster Autoscaler、Pod/Node の多層スケール
- ネットワーク/セキュリティ: GCLB/ILB、Network Policy、WAF/Cloud Armor、GKE Sandbox、Workload Identity
- エコシステム: Helm/Argo CD/Istio/Gateway API/Observability（Cloud Operations）
- Autopilot: Pod 指定のリソースに応じて自動で基盤最適化

## 欠点
- 学習/運用複雑性: Kubernetes の抽象化/周辺（Ingress、Service Mesh、CI/CD）の学習が必要
- デバッグ難度: 分散アーキで不具合の切り分けが難しい
- コスト可視化: Pod/Namespace/Shared リソースのコスト配賦が難しくなりやすい

## 価格の利点
- リソース効率: Bin Packing、HPA/VPA とスケールで無駄を削減
- Autopilot: Pod 単位課金でアイドルを抑制しやすい
- スポット/プリエンプト: ノードに Spot を混在させコスト圧縮

## 価格の欠点
- 周辺費用: ログ/メトリクス、LB、永続ディスク、イングレスの追加費用
- オーバーリクエスト: Requests/Limit 設計不備でコスト膨張
- Standard クラスタ: ノードアイドル時もVM費用が発生

## 選定の目安
- 向く: コンテナ前提、マルチサービス、継続デプロイ、ベンダーロック回避、SLO運用
- 代替: 単体Web/APIでイベント駆動中心→ Cloud Run、VM/特殊ドライバ→ GCE

## コスト最適化 Tips
- Requests/Limit を実測ベースで調整、VPA/HPA導入
- Autopilot を既定にしつつ、長時間CPUバウンドは Standard+Spot 併用
- Ingress はL7/L4の適材適所、ログのサンプリング/保持期間最適化
