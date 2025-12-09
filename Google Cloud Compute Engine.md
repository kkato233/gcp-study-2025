## Google Cloud Compute Engine 
Google Cloud Compute Engine の主な特徴:

仮想マシン（VM）を柔軟に提供（汎用/高CPU/高メモリ/GPU/ローカルSSDなどのマシンタイプ）
マネージドインスタンスグループ（MIG）でオートスケール・オートヒーリング・ローリング更新
永続ディスク（Standard/HDD, Balanced, SSD）とスナップショット、イメージでバックアップ・複製
VPC ネットワーク統合（ファイアウォール、ルート、Cloud NAT、Private Google Access）
負荷分散（HTTP(S)/TCP/UDP) とヘルスチェックによる高可用性構成
事前割り当て・スポットVMでコスト最適化、継続利用割引/コミットメント割引
OS 管理（OS Config/OS Patch/OS Inventory）と Shielded VM によるセキュリティ強化
起動スクリプト/メタデータ、カスタムイメージ、Instance Templates で再現性の高いデプロイ
IAM とサービスアカウントで細粒度のアクセス制御、Cloud Logging/Monitoring と連携
ディスク暗号化（デフォルト暗号化、Customer-Managed Encryption Keys に対応）
ハイブリッド接続（Cloud VPN/Interconnect）でオンプレと安全に連携
可観測性（Stackdriver Monitoring/Logging, メトリクス/ログ/トレースの収集）
近接配置/スケジュールポリシー、メンテナンスイベント通知、ライブマイグレーション（対応ホスト）
用途例:

Web/バッチ/高性能計算、データ処理基盤、コンテナランタイム（必要なら GKE/Container-Optimized OS）での実行
マイクロサービスのバックエンドを MIG＋ロードバランサでスケーラブルに運用


----------
Page 40 : Autohealing 用の HTTP ヘルスチェックを設定して 障害時にインスタンスが自動的に再作成される仕組みを構築できる
Page 52 : GKE では、既存クラスタに GPU 専用のノードプールを追加し、 対象 Pod で
        nodeSelector を用いてそのノードにスケジューリングさせる方法が最も効率的です。こ
        れにより、他のノードへの影響を避けつつ、 必要なリソースだけを追加できるため、コ
        ストパフォーマンスにも優れています。
Page 54 : サービスの停止なくスムーズに t2-standard-4 タイプのノードから x4-highmem-16 タイプに切り替え可能な方法は
        GKE のノードプール機能を使って既存クラスタに異なるマシンタイプのノードプール
        を追加すれば、既存サービスに影響を与えず新規 Pod を展開できます。 Podのリソース
        要求に基づき適切なノードへ自動スケジューリングされるため、 ゼロダウンタイムで拡
        張可能です。
-----------
GKE とは