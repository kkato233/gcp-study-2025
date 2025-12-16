# Google Professional Cloud Developer 練習問題（20問）

出題範囲は `study` フォルダの各まとめ（Cloud Run/Functions/PubSub/SQL/Spanner/Firestore/Bigtable/BigQuery/GKE/GCE/Dataflow/Dataproc/Datastream/Memorystore など）に基づく内容です。各問は解答と短い解説付きです。

---

## Q1. Cloud Run のトラフィック分割
単一の Cloud Run サービスで A/B/C を 33/33/34% で配信したい。最も簡潔な設計はどれか。

- A. 3つの別サービスを作成し、外部HTTP(S)ロードバランサで 33/33/34% に分割する
- B. 1サービスに3リビジョンをデプロイし、`services update-traffic` で割合分割を設定する
- C. 3リビジョンを作成するだけ（設定不要）
- D. 3リビジョンにタグを付け、タグURLをユーザーに配布する

解答: B

解説: Cloud Run は単一サービス内で複数リビジョンに対し割合分割が可能。リビジョン作成だけでは最新が100%を受け持つため、`update-traffic` が必要です。

---

## Q2. Cloud Run のユーザー単位の固定化
Cloud Run で簡易ABテストを行い、初回に割り当てたバージョンへ以後も同じユーザーを案内したい。追加のインフラは最小とする。妥当な方法はどれか。

- A. トラフィック分割だけを使う（ユーザー毎の固定は不要）
- B. タグ付きURLを用意し、Cookie でバリアントを保持してリダイレクトする
- C. 3サービスを作り、Serverless NEG と外部LBで Cookie ルーティングする
- D. Cloud Run では不可能

解答: B

解説: Cloud Run 単体にセッションスティッキーはないため、タグURL＋Cookie で疑似スティッキーが簡便。厳密制御が必要なら外部LB案（C）。

---

## Q3. Cloud Run Jobs の適用シナリオ
日次のバッチを実行し、HTTP受信は不要。最も適した選択はどれか。

- A. Cloud Run サービス（常時待受）
- B. Cloud Run Jobs
- C. Cloud Functions（HTTP トリガのみ）
- D. Pub/Sub サブスクライバ（Pull）

解答: B

解説: HTTP待受のないバッチには Cloud Run Jobs が適する。Scheduler や Workflows などから起動可能。

---

## Q4. Cloud Functions（2nd gen）の特徴
次のうち、Cloud Functions 2nd gen の特徴として正しいものはどれか。

- A. 独自の実行基盤で Cloud Run とは無関係
- B. Eventarc を介した多サービスからのイベント受信が可能
- C. 同時実行数の調整はできない
- D. 最小インスタンスは設定できない

解答: B

解説: 2nd gen は Cloud Run/Eventarc 基盤。実行/同時実行/最小インスタンス調整が可能になり、イベント連携が広い。

---

## Q5. Pub/Sub の配信セマンティクス
Cloud Pub/Sub で正しい説明はどれか。

- A. exactly-once 配信がデフォルト
- B. at-most-once 配信がデフォルト
- C. at-least-once 配信がデフォルトで重複受信がありうる
- D. グローバルな完全順序が保証される

解答: C

解説: Pub/Sub は少なくとも1回配信。重複と順序に注意し、冪等性や ordering key を設計で補完する。

---

## Q6. Pub/Sub の順序保証
メッセージをキー単位で順序処理したい。適切な仕組みはどれか。

- A. デフォルトで全体順序が保証される
- B. サブスクリプション数を1にすれば順序保証される
- C. ordering key を使用する
- D. デッドレターを使う

解答: C

解説: ordering key 単位で順序保証が可能。全体順序ではない点に注意。

---

## Q7. デッドレター設定の目的
Pub/Sub のデッドレターサブスクリプションの主目的はどれか。

- A. メッセージの圧縮
- B. 再試行の無限ループ化
- C. 再配信上限超過メッセージの隔離・後処理
- D. 順序保証の強化

解答: C

解説: 処理失敗が続くメッセージを別トピック/サブスクリプションに隔離し、分析や補正処理を行う。

---

## Q8. データベース選定（トランザクション/スケール）
強整合のトランザクションをグローバルにスケールし、水平分割やレプリケーションの運用負荷を最小化したい。最適な選択はどれか。

- A. Cloud SQL
- B. AlloyDB for PostgreSQL（単一リージョン）
- C. Cloud Spanner（マルチリージョン）
- D. Firestore（Datastoreモード）

解答: C

解説: Cloud Spanner は水平スケールする分散RDBで強整合・マルチリージョンに対応。大規模OLTPに適する。

---

## Q9. 分析ワークロードの選定
大量データの集計・分析（列指向/サーバレスDWH）に最適なのはどれか。

- A. Cloud Bigtable
- B. BigQuery
- C. Firestore
- D. Cloud SQL

解答: B

解説: BigQuery はサーバレスDWHで大規模分析に最適。Bigtable は低レイテンシKey-Value/ワイドカラム用途。

---

## Q10. ドキュメント指向の選択
柔軟なスキーマ、グローバルスケール、イベント連携とも相性がよいドキュメント型 NoSQL はどれか。

- A. Firestore
- B. Cloud SQL
- C. BigQuery
- D. Memorystore

解答: A

解説: Firestore はドキュメント型 NoSQL。イベント連携やモバイル/サーバレスと相性がよい。

---

## Q11. 高スループットKey-Value
時系列や大規模センサーデータの低レイテンシ読み書きに適するのはどれか。

- A. Cloud Bigtable
- B. Cloud SQL
- C. Firestore
- D. BigQuery

解答: A

解説: Bigtable はワイドカラム型で高スループット・低レイテンシなアクセスに適する。

---

## Q12. キャッシュレイヤ
アプリの読み取りパフォーマンスとレイテンシ改善のためにインメモリキャッシュを導入したい。適切なサービスはどれか。

- A. Memorystore
- B. Cloud SQL
- C. Datastream
- D. Dataflow

解答: A

解説: Memorystore（Redis/Memcached）はインメモリキャッシュ向けマネージドサービス。

---

## Q13. Dataflow と Dataproc の選定
完全マネージドのストリーミング処理（Beamベース）で運用負荷を抑えたい。最適な選択はどれか。

- A. Dataproc（マネージドHadoop/Sparkクラスター）
- B. Dataflow
- C. Cloud Run Jobs
- D. Pub/Sub Lite

解答: B

解説: Dataflow はApache Beamベースのフルマネージド バッチ/ストリーミング。

---

## Q14. Datastream の用途
RDB からの変更データキャプチャ(CDC)で、ほぼリアルタイムに BigQuery へ取り込みたい。最適な選択はどれか。

- A. Datastream
- B. Dataflow 単体
- C. Cloud Functions（定期実行）
- D. Pub/Sub

解答: A

解説: Datastream はソースDBの変更を取り込み、BigQuery 等へ連携できるCDC向けマネージドサービス。

---

## Q15. GKE を選ぶべきケース
サイドカーや特権コンテナ、複雑なネットワーク要件、長時間接続など、Podレベルの高度な制御が必要。最適な選択はどれか。

- A. Cloud Run
- B. Cloud Functions
- C. GKE
- D. Cloud Run Jobs

解答: C

解説: 高度なコンテナオーケストレーションや特殊要件は GKE が適する。Cloud Run はリクエスト駆動で制約がある。

---

## Q16. GCE を選ぶべきケース
カーネルモジュールや特殊ドライバが必要で、OSレベルから自由度が欲しい。最適な選択はどれか。

- A. Cloud Run
- B. GCE
- C. Cloud Functions
- D. Firestore

解答: B

解説: OS/ドライバレベルの制御は仮想マシン（GCE）が適任。

---

## Q17. Cloud Run のコスト最適化
コールドスタート緩和とコストのバランスを取りたい。適切な調整はどれか（複数選択）。

- A. 最小インスタンスを適切に設定
- B. 同時実行数を調整
- C. イメージを軽量化して起動時間短縮
- D. すべてリージョンを有効化して冗長化

解答: A, B, C

解説: 最小/最大インスタンスと同時実行、イメージ軽量化などが有効。無意味なリージョン有効化はコスト増の恐れ。

---

## Q18. VPC コネクタとリージョン
Cloud Run/Functions から VPC 内リソースへアクセスする。ネットワーク課金とレイテンシを抑える推奨はどれか。

- A. 任意のリージョンの VPC コネクタを使う
- B. 実行環境と同一リージョンの VPC コネクタを使う
- C. グローバルに1つの VPC コネクタを共有
- D. VPC コネクタは課金がないため任意

解答: B

解説: 同一リージョン配置で egress/レイテンシを抑制。Functions/Run のドキュメントでも同リージョン設計が推奨される。

---

## Q19. Cloud Run のトラフィック切替の性質
トラフィック分割で 0%→100% に切り替えた場合の挙動として正しいものはどれか。

- A. 旧リビジョンへ数時間は一部が流れる
- B. 設定直後に即時切替わる
- C. デプロイし直すまで切替わらない
- D. URLが変わる

解答: B

解説: Cloud Run のトラフィック分割は設定変更で即時切替可能。URLは同一サービスURLのまま。

---

## Q20. Pub/Sub Lite を検討すべき状況
予測可能なスループットでコストを抑えたい場合に適切な選択はどれか。

- A. Pub/Sub Lite
- B. 通常の Pub/Sub のみ
- C. Dataflow
- D. Cloud Run

解答: A

解説: スループットが予測可能なら Pub/Sub Lite でコスト最適化が可能。
