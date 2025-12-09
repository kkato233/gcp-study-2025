## GKE（Google Kubernetes Engine）とは、

Google Cloud が提供するフルマネージドな Kubernetes クラスタサービスです。コンテナ化したアプリを自動スケーリング、自己修復、ローリング更新などの機能で安定運用できます。

主なポイント:

マネージド制御プレーン（高可用なマスターの運用不要）
ノードプールで異なるマシンタイプ/GPU/スポットVMを混在運用
オートスケーリング（Cluster/Node/Horizontal Pod）
ネットワークポリシー、Workload Identity、Shielded/COS でセキュア運用
負荷分散、Ingress、Observability（Cloud Logging/Monitoring）連携
IaC（Terraform/Deployment Manager）、Artifact Registry と統合
選択箇所の補足（ノードプール機能）:

複数のノードプールを追加し、nodeSelector や taints/tolerations で Pod を適切なノードへ配置
例: GPU専用プールやメモリ特化プールを追加してゼロダウンタイムで拡張可能

--------

GKE が ゼロダウンタイムで拡張可能な理由と その設定のポイント

    マネージド制御プレーンによる安全なローリング更新（新旧ノード共存）

    ノードプール追加で異種マシンタイプ/GPUを横並び導入可能

    オートスケーリング（Cluster Autoscaler＋HPA）で需要に応じてPod/ノードを自動増減

    Readiness/Liveness/PodDisruptionBudget によりトラフィック切替と退避を制御

    Surge/Unavailable 設定でDeployment更新時の同時追加・削減数を調整

設定のポイント:

    ノードプール運用
    目的別（GPU/高メモリ/スポット）に複数プールを用意
    nodeSelector/affinity と taints/tolerations で適切なノードへ配置

    デプロイ更新
    Deployment の strategy を RollingUpdate にし、maxSurge > 0・maxUnavailable = 0 を推奨
    Readiness Probe を必ず設定し、準備完了前にトラフィックを流さない

    オートスケーリング
    HPA をCPU/メモリやカスタムメトリクスで設定
    Cluster Autoscaler をノードプールごとに有効化（最小/最大ノード数）

    退避とメンテナンス
    PodDisruptionBudget で最低稼働数を保証
    ノードアップグレード/排他時は cordon＋drain（GKE の自動ローリングアップグレードを利用）

    ネットワークとLB
    Ingress/Service のヘルスチェックとバックエンドの readiness を一致させる

    可用性設計
    Regional クラスタで複数ゾーンに分散
    ステートフルは StatefulSet＋PodAntiAffinity＋適切なPV 設計

これらを組み合わせることで、新規ノード追加やDeployment更新時も、稼働中Podを維持しつつトラフィックを段階的に切り替え、サービス停止を避けられます。

-----------
GKE（Kubernetes）の主なリソース例:

ワークロード系: Deployment、StatefulSet、DaemonSet、Job、CronJob、ReplicaSet
サービス公開: Service、Ingress、Gateway（GKE Gateway）
設定/機密: ConfigMap、Secret
ストレージ: PersistentVolume、PersistentVolumeClaim、StorageClass
スケジューリング制御: PodDisruptionBudget、PriorityClass、HorizontalPodAutoscaler、VerticalPodAutoscaler
セキュリティ/アイデンティティ: NetworkPolicy、PodSecurity（Admission）、Workload Identity（GKE）
ノード/クラスタ: NodePool（GKE）、Namespace、ResourceQuota、LimitRange
カスタム拡張: CustomResourceDefinition（CRD）
補足:

一般的なアプリは Deployment＋Service＋HPA が基本。
状態保持は StatefulSet＋PV/PVC。
ノード全配布は DaemonSet。
定期バッチは CronJob。

--------
GKE の重要知識（追加ポイント）:

運用モード

    Standard と Autopilot（AutopilotはPod単位課金・運用省力化、細かいノード制御は不可）
    リリースチャネル（Rapid/Regular/Stable）でアップグレード安定性を選択
    ノード自動アップグレード/自動修復、メンテナンスウィンドウ/除外期間の設定

セキュリティ

    Workload Identity で Pod→GCP API の権限付与（鍵不要、最推奨）
    NetworkPolicy で East-West トラフィック制御
    PodSecurity（Baseline/Restricted）により特権・ホストアクセス制限
    Shielded/COS、ノードイメージの選択（Container-Optimized OS/Ubuntu）

スケジューリングと可用性

    Requests/Limits と QoS（Guaranteed/Burstable/BestEffort）を正しく設定
    HPA/VPA/Cluster Autoscaler の組み合わせ、ノードプール別の最小/最大
    PodDisruptionBudget、優先度/プリエンプション、TopologySpreadConstraintsで分散配置
    Regional クラスタでマルチゾーン冗長（コントロールプレーン/ノード分散）

ネットワーク/公開

    Ingress（HTTP(S) LB）と Gateway API、NEGs（Container-native LB）で正確なヘルスチェック
    内部/外部ロードバランサ、MultiCluster Ingress/Gateway でマルチクラスタ公開
    Cloud NAT、Private Google Access、VPC-native (IP alias) を理解

ストレージ

    StorageClass と動的PVC、ゾーン/リージョン冗長ディスクの特性
    StatefulSet＋Readiness/Liveness、PodAntiAffinity で状態管理

画像/CI/CD/監視

    Artifact Registry を使用し、ノード/Podに roles/artifactregistry.reader を付与
    Cloud Build/Cloud Deploy と統合したローリング/ブルーグリーン運用
    Managed Prometheus、Cloud Logging/Monitoring、SLO/アラート設定

コスト/ガバナンス

    Autopilotの課金モデル（Podリソース単位）、Standardはノード課金＋割引
    ResourceQuota/LimitRange、Namespace分離でチームごとのガバナンス

ノードプール設計

    用途別プール（GPU/高メモリ/スポット）＋taints/tolerations、nodeSelector/affinity
    サージアップグレード/不可用率の調整でゼロダウンタイム更新

これらを押さえると、セキュアで可用性・コスト最適なGKE運用が可能です。
