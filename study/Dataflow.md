# Dataflow まとめ

## 概要
- 対象: マネージド Apache Beam（バッチ/ストリーミングの統一データ処理）
- 主用途: ETL/ELT、ストリーム処理、ウィンドウ/集約、Pub/Sub→BigQuery/GCS 連携、ML前処理

## 利点
- 統一モデル: 同一コードでバッチ/ストリーム、イベント時刻/処理時刻、トリガ/ウィンドウ
- マネージド: オートスケール、ストリーミング エンジン、Flex/Classic テンプレート
- 連携: Pub/Sub、BigQuery、GCS、Spanner、Bigtable 等のI/Oコネクタが豊富

## 欠点
- 学習曲線: Beamモデル（PCollection、Window、Trigger、State/Timer）の理解が必要
- デバッグ/運用: 分散処理のデバッグ、再処理/順序/重複排除の設計
- コールドスタート: バッチの初期スピンアップ時間

## 価格の利点
- スケールに応じた課金: vCPU/メモリ/ディスク/Shuffle/Streaming Engine の従量
- マネージドの効率: 自前クラスタ運用より管理コストを削減

## 価格の欠点
- 常時ストリーミング: 24/7稼働で費用が積み上がる
- 外部費用: Pub/Sub/BigQuery/Storage/ネットワーク費が別途加算

## 選定の目安
- 向く: 大規模ETL、リアルタイム集計、再処理/順序制御が必要なストリーム
- 代替: 単純なバッチは Cloud Run Jobs、軽量変換は BigQuery SQL/Looker で代替可能

## コスト最適化 Tips
- オートスケール/ワーカタイプの適正化、Dataflow Shuffle/Streaming Engine の活用
- ウィンドウ/トリガの設計で重複/再処理最小化、I/Oを同リージョンに配置
- テンプレート化で再利用と起動時間短縮、ログ保持期間を最適化
