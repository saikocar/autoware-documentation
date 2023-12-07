コンポーネントの一時的なパフォーマンス メトリックの定義
一時的なパフォーマンス指標を定義する動機
ページの目的
このページでは、Autoware のコンポーネントの一時的なパフォーマンスを評価するためのメトリックを定義するポリシーを紹介します。「時間的パフォーマンス」という用語は、精度とも呼ばれる機能的パフォーマンスと時間関連のパフォーマンスを区別するために、ページ全体でよく使用されます。

Autoware に採用されているほとんどのアルゴリズムは、可能な限り高い頻度で、短い応答時間で実行されることが予想されます。安全な自動運転を実現するためには、認識される状況と実際の状況との間に時間差がないことが望ましい結果の 1 つです。時間のギャップは一般に遅延と呼ばれます。遅延が大きい場合、システムは古い状況に基づいて軌道と操縦を決定する場合があります。したがって、遅延により実際の状況が認識されている状況と異なる場合、システムは予期しない決定を下す可能性があります。

上で述べたように、このページではメトリクスを定義するためのポリシーを示します。さらに、このページには、Autoware の主な機能 (ローカリゼーション、認識、計画、制御) にとって重要なサンプル メトリックのリストが含まれています。

!!! 注記

Other functionalities, such as system components for diagnosis, are excluded currently. However they will be taken into account in the near future.
一時的なパフォーマンス指標の寄与
Autoware を評価するには、一時的なパフォーマンス メトリックが重要です。これらのメトリクスは、新しいアルゴリズムやロジックによって引き起こされる遅延を評価するのに特に役立ちます。これらは、車両の統合段階で、デスクトップ コンピュータ上のソフトウェアの一時的なパフォーマンスと車両上のソフトウェアのパフォーマンスを比較するときに使用できます。

さらに、これらの指標は、ミドルウェア、オペレーティング システム、コンピューターの設計者や評価者にとっても役立ちます。これらはユーザーと製品の要件に基づいて選択されます。これらの要件の 1 つは、Autoware の実行に十分な一時的なパフォーマンスを提供することです。「十分な一時的な性能」は一時的な性能要件として定義されますが、製品の種類や運用設計ドメイン (ODD) などの要因によって異なるため、要件を定義するのは困難な場合があります。次に、このページでは、要件ではなく一時的なパフォーマンス メトリクスに特に焦点を当てます。

Autoware の信頼性を評価するには、一時的なパフォーマンス メトリックが重要です。ただし、Autoware の信頼性を確保するには、一時的なパフォーマンス指標だけでなく、他の指標も考慮する必要があります。

メトリクスを評価するためのツール
ページにリストされている指標に従って Autoware を評価するために利用できるツールがいくつかあります。たとえば、CARETとros2_tracing は、Linux および ROS 2 で Autoware を評価するときに推奨されるオプションです。これらのツールのいずれかを使用してメトリクスを測定する場合は、対応するユーザー ガイドの手順を参照してください。Autoware を Linux および ROS 2 以外のプラットフォームにインポートする場合は、評価用にサポートされているツールを選択する必要があることに注意することが重要です。

!!! 注記

TIER IV plans to measure Autoware, which is running according to [the tutorial](../../tutorials/), and provide a performance evaluation report periodically. An example of such a report can be found [here](https://tier4.github.io/CARET_report/), although it may not include all of the metrics listed.
このページは、これらのツールの使用方法や指標の測定方法について説明することを目的としたものではありません。使用される特定のツールよりもメトリクス自体が重要であるため、そのメトリクス自体に主に焦点を当てています。これらの指標は、使用されているプラ​​ットフォームに関係なく、その関連性を維持します。

一時的なパフォーマンス指標を定義するポリシー
前述したように、Autoware の構成は製品タイプ、ODD、その他の要因によって異なります。構成が多様であるため、Autoware を評価するための統一的な指標を定義することが困難になります。
ただし、それらを定義するために使用されたポリシーは、構成が変更された場合でも基本的に再利用されます。各時間パフォーマンス メトリクスは、実行頻度と応答時​​間の 2 つのタイプに分類されます。通信遅延など、さまざまなタイプのメトリクスがありますが、簡単にするために 2 つのタイプのみを考慮します。
実行頻度は、プロセス間通信 (IPC) メッセージの速度を使用して観察されます。Autoware には膨大な数のメッセージが表示されますが、すべてに対処する必要はありません。一部のメッセージは機能にとって重要である可能性があるため、評価のために選択する必要があります。
応答時間は一連の処理にかかる時間です。一連の処理をパスと呼びます。応答時間は、パスの開始と終了のタイムスタンプから計算されます。Autoware では多くのパスを定義できますが、重要なパスを選択する必要があります。

メトリックを選択するためのヒントとして、メッセージとパスのいくつかの特性を次に示します。

センサーからの観測値が消費される境界上のメッセージとパス
機能の境界（認識と計画の境界など）上のメッセージとパス
タイマーベースの周波数が切り替わる境界上のメッセージとパス
2 つの異なるメッセージが同期およびマージされる境界上のメッセージとパス
予想される頻度で送信する必要があるメッセージ (車両コマンド メッセージなど)
これらのヒントはほとんどの構成に役立ちますが、除外される場合もあります。メトリクスを正確に定義するには、構成を理解する必要があります。

さらに、アーキテクチャ レベルから詳細設計および実装レベルに至るまで、指標を段階的に決定することをお勧めします。さまざまな粒度レベルでメトリクスを混合すると、混乱が生じる可能性があります。

サンプルメトリクスのリスト
このセクションでは、説明されているポリシーに従ってメトリクスを定義する方法を示し、チュートリアルに従って起動される Autoware のメトリクスのリストを示します。このセクションは複数のサブセクションに分かれており、各サブセクションにはモデル図と、重要な一時的なパフォーマンス メトリックを説明する付随リストが含まれています。各モデルには、これらの指標の指標となるチェックポイントが装備されています。

最初のサブセクションでは、トップレベルの一時的なパフォーマンス メトリックを示します。これらは、Autoware 全体の抽象構造で表されます。詳細なメトリクスはモデルが複雑になるため、モデルには含まれていません。代わりに、後続のセクションで詳細なメトリクスを紹介します。詳細なメトリクスは、トップレベルのメトリクスと比較してより頻繁に更新される可能性があるため、これらを個別に分類するもう 1 つの理由があります。

各リストには基準値の列が含まれています。基準値は、チュートリアルに従って Autoware を実行したときの各メトリックの観測値を表します。参照値は必須の値ではないことに注意することが重要です。つまり、特定のメトリックが参照値を満たさない場合でも、 Autoware がチュートリアルの実行に必ずしも失敗するわけではありません。

Autoware のトップレベルの一時的なパフォーマンス メトリック
以下の図は、トップレベルの時間パフォーマンス メトリクスのモデルを示しています。

トップレベルの時間的パフォーマンス指標のモデル

次の 3 つのポリシーは、トップレベルのパフォーマンス メトリックの選択に役立ちます。

センサーデータなどの観測値を消費するコンポーネントに基づいて Autoware を分割し、これらのコンポーネントの処理頻度と応答時​​間を考慮します。
計画と管理の入口点に基づいて Autoware を分割し、これらのコンポーネントを中心に処理頻度と応答時​​間を考慮します
対象車両によって異なる可能性があるため、車両インターフェースの最小メトリクスを表示します。
さらに、アルゴリズムは複数のノードとして実装され、パイプライン処理システムとして機能することが想定されています。

ID	モデルでの表現	メトリクスの意味	関連機能	基準値	指標として選択する理由	注記
AWOV-001	CPA #9 から CPA #18 へのメッセージ レート	予測から計画への結果の更新速度。	感知	10Hz	計画は、正確な軌道を作成するために、Perception からの新鮮で最新の知覚データに依存します。	
AWOV-002	CPA #0 から CPA #18 を経由して CPA #20 までの応答時間	Perception本体の応答時間。	感知	該当なし	計画は、正確な軌道を作成するために、Perception からの新鮮で最新の知覚データに依存します。	このメトリックは、トラッキングで遅延補正が無効になっている場合に使用されます。
AWOV-003	CPA #7 から CPA #20 までの応答時間	Tracking の Tracking 出力から Planning でのデータ消費までの応答時間。	感知	該当なし	計画は、正確な軌道を作成するために、Perception からの新鮮で最新の知覚データに依存します。	このメトリックは、トラッキングで遅延補正が有効になっている場合に使用されます。
AWOV-004	CPA #0 から CPA #6 までの応答時間	センシングと検出で点群データを処理する期間。	感知	該当なし	追跡は、正確な追跡のためにリアルタイムで最新の検知データを提供する検出に依存しています。	このメトリックは、トラッキングで遅延補正が有効になっている場合に使用されます。
AWOV-005	CPA #4 から CPA #5 へのメッセージ レート	トラッキングによって受信された検出結果の更新レート。	感知	10Hz	追跡は、正確な追跡のためにリアルタイムで最新の検知データを提供する検出に依存しています。	
AWOV-006	CPA #0 から CPA #14 までの応答時間	LiDAR からの観測データの出力から、NDT Scan Matcher を介した EKF Localizer でのデータの消費までの応答時間。	ローカリゼーション	該当なし	EKF Localizer は、自己姿勢を正確に推定するために、センサーからの新鮮かつ最新の観察データに依存しています。	
AWOV-007	CPA #11 から CPA #13 へのメッセージ レート	NDT Scan Matcher によって推定されたポーズのレートを更新します。	ローカリゼーション	10Hz	EKF Localizer は、自己姿勢を正確に推定するために、センサーからの新鮮かつ最新の観察データに依存しています。	
AWOV-008	CPA #15 から CPA #12 へのメッセージ レート	EKF Localizer によって推定されたフィードバックに基づく姿勢の更新レート。	ローカリゼーション	50Hz	NDT スキャン マッチャーは、線形補間のために EKF Localizer から推定ポーズをスムーズに受け取ることに依存しています。	
AWOV-009	CPA #17 から CPA #19 へのメッセージ レート	Planning が受信したローカリゼーション結果の更新速度。	ローカリゼーション	50Hz	計画はローカリゼーションに依存して、推定されたポーズを頻繁に更新します。	
AWOV-010	CPA #20 から CPA #23 への応答時間	Planning の開始から Control での Trajectory メッセージの消費までの処理時間。	企画	該当なし	車両は、安全な運転動作を実現するために、短期間で軌道を更新するための計画に依存しています。	
AWOV-011	CPA #21 から CPA #22 へのメッセージ レート	計画からの軌跡メッセージの更新レート。	企画	10Hz	車両は、安全な運転動作を実現するために計画を利用して軌道を頻繁に更新します。	
AWOV-012	CPA #24 から CPA #25 へのメッセージ レート	制御コマンドの更新レート。	コントロール	33Hz	コントロールの安定性と快適さは、コントロールのサンプリング周波数に依存します。	
AWOV-013	CPA #26 と車両間のメッセージ レート	Autoware と Vehicle 間の通信速度。	車両インターフェース	該当なし	車両は、Autoware があらかじめ決められた頻度で相互に通信する必要があります。	一時的な性能要件は車両のタイプによって異なります。
!!! 注記

There is an assumption that each of sensors, such as LiDARs and cameras, outputs a set of pointcloud with a timestamp. CPA #0 is observed with the timestamp. If the sensors are not configured to output the timestamp, the time when Autoware receives the pointcloud is used instead. That is represented by CPA #1 in the model. The detailed metrics employs the idea as well.
知覚の詳細な時間パフォーマンス指標
以下の図は、知覚の時間パフォーマンス メトリクスのモデルを示しています。

知覚時間パフォーマンス指標のモデル

次の 2 つのポリシーは、パフォーマンス メトリックの選択に役立ちます。

プランニングにおける物体認識と信号機認識の認識結果が消費される頻度と応答時​​間について
複数の処理パスからのデータのマージポイントで知覚コンポーネントを分割し、そのポイント付近の頻度と応答時​​間を考慮します。
次のリストは、知覚の一時的なパフォーマンス メトリックを示しています。

ID	モデルでの表現	メトリクスの意味	関連機能	基準値	指標として選択する理由	注記
APER-001	CPP #2 から CPP #26 までのメッセージ レート	信号認識の更新速度。	信号機の認識	10Hz	計画は、正確な意思決定を行うために、信号認識から得られる新鮮で最新の認識データに依存します。	
APER-002	CPP #0 から CPP #30 までの応答時間	カメラ入力からプランニングでの結果の消費までの応答時間。	信号機の認識	該当なし	計画は、正確な意思決定を行うために、信号認識から得られる新鮮で最新の認識データに依存します。	
APER-003	CPP #25 から CPP #28 へのメッセージ レート	予測（物体認識）から計画への結果の更新率。	物体認識	10Hz	計画は、正確な軌道を作成するために、Perception からの新鮮で最新の知覚データに依存します。	メトリックはAWOV-001と同じです。
APER-004	CPP #6 から CPP #30 までの応答時間	Tracking の Tracking 出力から Planning でのデータ消費までの応答時間。	物体認識	該当なし	計画は、正確な軌道を作成するために、Perception からの新鮮で最新の知覚データに依存します。	このメトリクスは AWOV-002 と同じで、トラッキングで遅延補正が無効になっている場合に使用されます。
APER-005	CPP #23 から CPP #30 までの応答時間	Tracking の Tracking 出力から Planning でのデータ消費までの応答時間。	物体認識	該当なし	計画は、正確な軌道を作成するために、Perception からの新鮮で最新の知覚データに依存します。	このメトリクスは AWOV-003 と同じで、トラッキングで遅延補正が有効になっている場合に使用されます。
APER-006	CPP #6 から CPP #21 までの応答時間	センシングと検出で点群データを処理する期間。	物体認識	該当なし	追跡は検出に依存して、リアルタイムかつ最新の認識データを提供します。	メトリクスは AWOV-004 と同じで、トラッキングで遅延補正が有効になっている場合に使用されます。
APER-007	CPP #20 から CPP #21 へのメッセージ レート	トラッキングによって受信された検出結果の更新レート。	物体認識	10Hz	追跡は、正確な追跡のためにリアルタイムで最新の検知データを提供する検出に依存しています。	メトリックはAWOV-005と同じです
APER-008	CPP #14 から CPP #19 へのメッセージ レート	Sensor Fusion から送信されるデータの更新レート。	物体認識	10Hz	Association Merger は、データ同期のために予想される頻度で更新されるデータに依存します。	
APER-009	CPP #16 から CPP #19 へのメッセージ レート	トラッカーによる検出から送信されるデータの更新レート。	物体認識	10Hz	Association Merger は、データ同期のために予想される頻度で更新されるデータに依存します。	
APER-010	CPP #18 から CPP #19 へのメッセージ レート	Validation から送信されるデータの更新レート	オブジェクトの認識。	10Hz	Association Merger は、データ同期のために予想される頻度で更新されるデータに依存します。	
APER-011	CPP #6 から CPP #14 を経由して CPP #19 までの応答時間	LiDAR がポイントクラウドを出力した後に Sensor Fusion から送信されたデータを消費するための応答時間。	物体認識	該当なし	Association Merger は、データ同期のために新鮮な最新のデータに依存します。	
APER-012	CPP #6 から CPP #16 を経由して CPP #19 までの応答時間	LiDAR が点群を出力した後、Tracker による検出から送信されたデータを消費するための応答時間。	物体認識	該当なし	Association Merger は、データ同期のために新鮮な最新のデータに依存します。	
APER-013	CPP #6 から CPP #18 を経由して CPP #19 までの応答時間	LiDAR がポイントクラウドを出力した後、Validator から送信されたデータを消費するための応答時間。	物体認識	該当なし	Association Merger は、データ同期のために新鮮な最新のデータに依存します。	
APER-014	CPP #10 から CPP #13 へのメッセージ レート	クラスタリングから送信されるデータの更新速度。	物体認識	10Hz	Sensor Fusion は、データ同期のために予想される頻度で更新されるデータに依存します。	
APER-015	CPP #5 から CPP #13 までのメッセージ レート	カメラベースのオブジェクト検出から送信されるデータの更新レート。	物体認識	10Hz	Sensor Fusion は、データ同期のために予想される頻度で更新されるデータに依存します。	
APER-016	CPP #6 から CPP #13 までの応答時間	LiDAR が点群を出力した後にクラスタリングから送信されたデータを消費するための応答時間。	物体認識	該当なし	Sensor Fusion は、データ同期のために新鮮な最新データに依存します。	
APER-017	CPP #3 から CPP #13 までの応答時間	カメラが画像を出力した後、カメラベースのオブジェクト検出から送信されたデータを消費するための応答時間。	物体認識	該当なし	Sensor Fusion は、データ同期のために新鮮な最新データに依存します。	
APER-018	CPP #10 から CPP #17 へのメッセージ レート	クラスタリングから送信されるデータの更新速度。	物体認識	10Hz	バリデーターは、データ同期のために予想される頻度で更新されるデータに依存します。	APER-014 に似ていますが、トピックのメッセージが異なります。
APER-019	CPP #12 から CPP #17 へのメッセージ レート	DNN ベースのオブジェクト認識から送信されるデータの更新レート。	物体認識	10Hz	バリデーターは、データ同期のために予想される頻度で更新されるデータに依存します。	
APER-020	CPP #6 から CPP #10 を経由して CPP #17 までの応答時間	LiDAR が点群を出力した後にクラスタリングから送信されたデータを消費するための応答時間。	物体認識	該当なし	バリデーターは、データの同期のために最新の更新日のデータに依存します。	APER-015 に似ていますが、トピックのメッセージが異なります。
APER-021	CPP #6 から CPP #12 を経由した CPP #17 までの応答時間	LiDAR が点群を出力した後、DNN ベースの物体認識から送信されたデータを消費するための応答時間。	物体認識	該当なし	バリデーターは、データの同期のために最新の更新日のデータに依存します。	
障害物のセグメンテーションと計画の間のパスに関する詳細な時間パフォーマンス メトリクス
Perception の重要な部分である障害物のセグメンテーションは、データを Planning に送信します。以下の図は、障害物のセグメンテーションと計画に関連するパフォーマンス メトリックを考慮したモデルを示しています。

障害物セグメンテーションの時間パフォーマンス指標のモデル

!!! 注記

Both the Obstacle grid map and Obstacle segmentation transmit data to multiple sub-components of Planning. However, not all of these sub-components are described in the model. This is because our primary focus is on the paths from LiDAR to Planning via Obstacle segmentation.
次のリストは、障害物のセグメンテーションと計画に関する一時的なパフォーマンス メトリックを示しています。

ID	モデルでの表現	メトリクスの意味	関連機能	基準値	指標として選択する理由	注記
OSEG-001	CPS #4 から CPS #7 までのメッセージ レート	計画によって受信された占有グリッド マップの更新率 ( behavior_path_planner)	障害物のセグメンテーション	10Hz	計画は、正確な軌道を作成するために頻繁かつスムーズに更新される占有グリッド マップに依存します。	
OSEG-002	CPS #0 から CPS #7 を経由して CPS #9 までの応答時間	LiDAR がセンシング データを出力した後、占有グリッド マップを消費するまでの応答時間。	障害物のセグメンテーション	該当なし	計画は、正確な軌道を作成するために、占有グリッド マップからの新鮮で最新の認識データに依存します。	
OSEG-003	CPS #6 から CPS #11 までのメッセージ レート	計画によって受信された障害物セグメント化の更新レート ( behavior_velocity_planner)。	障害物のセグメンテーション	10Hz	計画は、正確な軌道を作成するために頻繁かつスムーズに更新される障害物セグメンテーションに依存しています。	
OSEG-004	CPS #0 から CPS #11 を経由して CPS #13 までの応答時間	LiDAR がセンシング データを出力した後、障害物セグメンテーションを消費するまでの応答時間。	障害物のセグメンテーション	該当なし	計画は、正確な軌道を作成するために、障害物セグメンテーションからの新鮮で最新の知覚データに依存します。
# Defining temporal performance metrics on components

## Motivation to defining temporal performance metrics

### Objective of the page

This page introduces policies to define metrics to evaluate temporal performance on components of Autoware. The term "temporal performance" is often used throughout the page in order to distinguish between functional performance, which referred to as accuracy as well, and time-related performance.

It is expected that most algorithms employed for Autoware are executed with as high frequency and short response time as possible. In order to achieve safe autonomous driving, one of the desired outcomes is no time gap between perceived and actual situation. The time gap is commonly referred to as delay. If the delay is significant, the system may determine trajectory and maneuver based on outdated situation. Consequently, if the actual situation differs from the perceived one due to the delay, the system may make unexpected decisions.

As mentioned above, this page presents the policies to define metrics. Besides, the page contains lists of sample metrics that are crucial for the main functionalities of Autoware: Localization, Perception, Planning, and Control.

!!! note

    Other functionalities, such as system components for diagnosis, are excluded currently. However they will be taken into account in the near future.

### Contribution of the temporal performance metrics

Temporal performance metrics are important for evaluating Autoware. These metrics are particularly useful for assessing delays caused by new algorithms and logic. They can be employed when comparing the temporal performance of software on a desktop computer with that on a vehicle during the vehicle integration phase.

In addition, these metrics are useful for designers and evaluators of middleware, operating systems, and computers. They are selected based on user and product requirements. One of these requirements is to provide sufficient temporal performance for executing Autoware. "Sufficient temporal performance" is defined as a temporal performance requirement, but it can be challenging to define the requirement because it varies depending on the product type, Operational Design Domain (ODD), and other factors. Then, this page specifically focuses on temporal performance metrics rather than requirements.

Temporal performance metrics are important for evaluating the reliability of Autoware. However, ensuring the reliability of Autoware requires consideration of not only temporal performance metrics but also other metrics.

### Tools for evaluating the metrics

There are several tools available for evaluating Autoware according to the metrics listed in the page. For example, both [CARET](https://github.com/tier4/caret) and [ros2_tracing](https://github.com/ros2/ros2_tracing) are recommended options when evaluating Autoware on Linux and ROS 2. If you want to measure the metrics with either of these tools, refer to the corresponding user guide for instructions. It's important to note that if you import Autoware to a platform other than Linux and ROS 2, you will need to choose a supported tool for evaluation.

!!! note

    TIER IV plans to measure Autoware, which is running according to [the tutorial](../../tutorials/), and provide a performance evaluation report periodically. An example of such a report can be found [here](https://tier4.github.io/CARET_report/), although it may not include all of the metrics listed.

The page does not aim to provide instructions on how to use these tools or measure the metrics. Its primary focus is on the metrics themselves, as they are more important than the specific tools used. These metrics retain their relevance regardless of the employed platform.

## Policies to define temporal performance metrics

As mentioned above, the configuration of Autoware varies by the product type, ODD, and other factors. The variety of configurations makes it difficult to define the uniform metrics for evaluating Autoware.  
However, the policies used to define them are basically reused even when the configuration changes. Each of temporal performance metrics is categorized into two types: **execution frequency** and **response time**. Although there are many types of metrics, such as communication latency, the only two types are considered for simplicity.  
Execution frequency is observed using rate of Inter-Process Communication (IPC) messages. You will find an enormous number of messages in Autoware, but you don't have to take care of all. Some messages might be critical to functionality and they should be chosen for evaluation.  
Response time is duration elapsed through a series of processing. A series of processing is referred to as a path. Response time is calculated from timestamps of start and end of a path. Although many paths can be defined in Autoware, you have to choose significant paths.

As a hint, here are some characteristics of message and path in order to choose metrics.

1. Messages and paths on boundaries where observed values from sensors are consumed
2. Messages and paths on boundaries of functions, e.g., a boundary of perception and planning
3. Messages and paths on boundaries where timer-based frequency is switched
4. Messages and paths on boundaries where two different messages are synchronized and merged
5. Messages that must be transmitted at expected frequency, e.g., vehicle command messages

Those hints would be helpful for most configurations but there may be exclusions. Defining metrics precisely requires an understanding of configuration.

In addition, it is recommended that metrics be determined incrementally from the architectural level to the detailed design and implementation level. Mixing metrics at different levels of granularity can be confusing.

## List of sample metrics

This section demonstrates how to define metrics according to the policies explained and has lists of the metrics for Autoware launched according to [the tutorial](../../tutorials/). The section is divided into multiple subsections, each containing a model diagram and an accompanying list that explains the important temporal performance metrics. Each model is equipped with checkpoints that serve as indicators for these metrics.

The first subsection presents the top-level temporal performance metrics, which are depicted in the abstract structure of Autoware as a whole. The detailed metrics are not included in the model as they would add complexity to it. Instead, the subsequent section introduces the detailed metrics. The detailed metrics are subject to more frequent updates compared to the top-level ones, which is another reason for categorizing them separately.

Each list includes a column for the reference value. The reference value represents the observed value of each metric when Autoware is running according to [the tutorial](../../tutorials/). It is important to note that the reference value is not a required value, meaning that Autoware does not necessarily fail in [the tutorial](../../tutorials/) execution if certain metrics do not fulfill the reference value.

### Top-level temporal performance metrics for Autoware

The diagram below introduces the model for top-level temporal performance metrics.

![Model for top-level temporal performance metrics](./images/important-temporal-performance-metrics/model-for-top-level-metrics.svg)

The following three policies assist in selecting the top-level performance metrics:

- Splitting Autoware based on components that consume observed values, such as sensor data, and considering the processing frequency and response time around these components
- Dividing Autoware based on the entry point of Planning and Control and considering the processing frequency and response time around these components
- Showing the minimum metrics for the Vehicle Interface, as they may vary depending on the target vehicle

Additionally, it is assumed that algorithms are implemented as multiple nodes and function as a pipeline processing system.

<!-- cspell: ignore AWOV OSEG -->

| ID       | Representation in the model                          | Metric meaning                                                                                                   | Related functionality | Reference value | Reason to choose it as a metric                                                                               | Note                                                               |
| -------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------- | --------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| AWOV-001 | **Message rate** from CPA #9 to CPA #18              | Update rate of result from Prediction to Planning.                                                               | Perception            | 10 Hz           | Planning relies on fresh and up-to-date perceived data from Perception for creating accurate trajectory.      |                                                                    |
| AWOV-002 | **Response time** from CPA #0 to CPA #20 via CPA #18 | Response time in main body of Perception.                                                                        | Perception            | N/A             | Planning relies on fresh and up-to-date perceived data from Perception for creating accurate trajectory.      | The metric is used if delay compensation is disabled in Tracking.  |
| AWOV-003 | **Response time** from CPA #7 to CPA #20             | Response time from Tracking output of Tracking to its data consumption in Planning.                              | Perception            | N/A             | Planning relies on fresh and up-to-date perceived data from Perception for creating accurate trajectory.      | The metric is used if delay compensation is enabled in Tracking.   |
| AWOV-004 | **Response time** from CPA #0 to CPA #6              | Duration to process pointcloud data in Sensing and Detection.                                                    | Perception            | N/A             | Tracking relies on detection to provide real-time and up-to-date sensed data for accurate tracking.           | The metric is used if delay compensation is enabled in Tracking.   |
| AWOV-005 | **Message rate** from CPA #4 to CPA #5               | Update rate of Detection result received by Tracking.                                                            | Perception            | 10 Hz           | Tracking relies on detection to provide real-time and up-to-date sensed data for accurate tracking.           |                                                                    |
| AWOV-006 | **Response time** from CPA #0 to CPA #14             | Response time from output of observed data from LiDARs to its consumption in EKF Localizer via NDT Scan Matcher. | Localization          | N/A             | EKF Localizer relies on fresh and up-to-date observed data from sensors for accurate estimation of self pose. |                                                                    |
| AWOV-007 | **Message rate** from CPA #11 to CPA #13             | Update rate of pose estimated by NDT Scan Matcher.                                                               | Localization          | 10 Hz           | EKF Localizer relies on fresh and up-to-date observed data from sensors for accurate estimation of self pose. |                                                                    |
| AWOV-008 | **Message rate** from CPA #15 to CPA #12             | Update rate of feed backed pose estimated by EKF Localizer.                                                      | Localization          | 50 Hz           | NDT Scan Matcher relies on receiving estimated pose from EKF Localizer smoothly for linear interpolation.     |                                                                    |
| AWOV-009 | **Message rate** from CPA #17 to CPA #19             | Update rate of Localization result received by Planning.                                                         | Localization          | 50 Hz           | Planning relies on Localization to update the estimated pose frequently.                                      |                                                                    |
| AWOV-010 | **Response time** from CPA #20 to CPA #23            | Processing time from beginning of Planning to consumption of Trajectory message in Control.                      | Planning              | N/A             | A vehicle relies on Planning to update trajectory within a short time frame to achieve safe driving behavior. |                                                                    |
| AWOV-011 | **Message rate** from CPA #21 to CPA #22             | Update rate of Trajectory message from Planning.                                                                 | Planning              | 10 Hz           | A vehicle relies on Planning to update trajectory frequently to achieve safe driving behavior.                |                                                                    |
| AWOV-012 | **Message rate** from CPA #24 to CPA #25             | Update rate of Control command.                                                                                  | Control               | 33 Hz           | Control stability and comfort relies on sampling frequency of Control.                                        |                                                                    |
| AWOV-013 | **Message rate** between CPA #26 and Vehicle         | Communication rate between Autoware and Vehicle.                                                                 | Vehicle Interface     | N/A             | A vehicle requires Autoware to communicate with each other at predetermined frequency.                        | Temporal performance requirement varies depending on vehicle type. |

!!! note

    There is an assumption that each of sensors, such as LiDARs and cameras, outputs a set of pointcloud with a timestamp. CPA #0 is observed with the timestamp. If the sensors are not configured to output the timestamp, the time when Autoware receives the pointcloud is used instead. That is represented by CPA #1 in the model. The detailed metrics employs the idea as well.

### Detailed temporal performance metrics for Perception

The diagram below introduces the model for temporal performance metrics for Perception.

![Model for Perception temporal performance metrics](./images/important-temporal-performance-metrics/model-for-perception-metrics.svg)

The following two policies assist in selecting the performance metrics:

- Regarding the frequency and response time at which Recognition results from Object Recognition and Traffic Light Recognition are consumed in Planning
- Splitting Perception component on merging points of data from multiple processing paths and considering the frequency and response time around that point

The following list shows the temporal performance metrics for Perception.

| ID       | Representation in the model                          | Metric meaning                                                                                       | Related functionality     | Reference value | Reason to choose it as a metric                                                                                     | Note                                                                                   |
| -------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| APER-001 | **Message rate** from CPP #2 to CPP #26              | Update rate of Traffic Light Recognition.                                                            | Traffic Light Recognition | 10 Hz           | Planning relies on fresh and up-to-date perceived data from Traffic Light Recognition for making precise decisions. |                                                                                        |
| APER-002 | **Response time** from CPP #0 to CPP #30             | Response time from camera input to consumption of the result in Planning.                            | Traffic Light Recognition | N/A             | Planning relies on fresh and up-to-date perceived data from Traffic Light Recognition for making precise decisions. |                                                                                        |
| APER-003 | **Message rate** from CPP #25 to CPP #28             | Update rate of result from Prediction (Object Recognition) to Planning.                              | Object Recognition        | 10 Hz           | Planning relies on fresh and up-to-date perceived data from Perception for creating accurate trajectory.            | The metric is same as AWOV-001.                                                        |
| APER-004 | **Response time** from CPP #6 to CPP #30             | Response time from Tracking output of Tracking to its data consumption in Planning.                  | Object Recognition        | N/A             | Planning relies on fresh and up-to-date perceived data from Perception for creating accurate trajectory.            | The metric is same as AWOV-002 and used if delay compensation is disabled in Tracking. |
| APER-005 | **Response time** from CPP #23 to CPP #30            | Response time from Tracking output of Tracking to its data consumption in Planning.                  | Object Recognition        | N/A             | Planning relies on fresh and up-to-date perceived data from Perception for creating accurate trajectory.            | The metric is same as AWOV-003 and used if delay compensation is enabled in Tracking.  |
| APER-006 | **Response time** from CPP #6 to CPP #21             | Duration to process pointcloud data in Sensing and Detection.                                        | Object Recognition        | N/A             | Tracking relies on Detection to provide real-time and up-to-date perceived data.                                    | The metrics is same as AWOV-004 and used if delay compensation is enabled in Tracking. |
| APER-007 | **Message rate** from CPP #20 to CPP #21             | Update rate of Detection result received by Tracking.                                                | Object Recognition        | 10 Hz           | Tracking relies on detection to provide real-time and up-to-date sensed data for accurate tracking.                 | The metric is same as AWOV-005                                                         |
| APER-008 | **Message rate** from CPP #14 to CPP #19             | Update rate of data sent from Sensor Fusion.                                                         | Object Recognition        | 10 Hz           | Association Merger relies on the data to be updated at expected frequency for data synchronization.                 |                                                                                        |
| APER-009 | **Message rate** from CPP #16 to CPP #19             | Update rate of data sent from Detection by Tracker.                                                  | Object Recognition        | 10 Hz           | Association Merger relies on the data to be updated at expected frequency for data synchronization.                 |                                                                                        |
| APER-010 | **Message rate** from CPP #18 to CPP #19             | Update rate of data sent from Validation                                                             | Object Recognition.       | 10 Hz           | Association Merger relies on the data to be updated at expected frequency for data synchronization.                 |
| APER-011 | **Response time** from CPP #6 to CPP #19 via CPP #14 | Response time to consume data sent from Sensor Fusion after LiDARs output pointcloud.                | Object Recognition        | N/A             | Association Merger relies on fresh and up-to-date data for data synchronization.                                    |                                                                                        |
| APER-012 | **Response time** from CPP #6 to CPP #19 via CPP #16 | Response time to consume data sent from Detection by Tracker after LiDARs output pointcloud.         | Object Recognition        | N/A             | Association Merger relies on fresh and up-to-date data for data synchronization.                                    |                                                                                        |
| APER-013 | **Response time** from CPP #6 to CPP #19 via CPP #18 | Response time to consume data sent from Validator after LiDARs output pointcloud.                    | Object Recognition        | N/A             | Association Merger relies on fresh and up-to-date data for data synchronization.                                    |                                                                                        |
| APER-014 | **Message rate** from CPP #10 to CPP #13             | Update rate of data sent from Clustering.                                                            | Object Recognition        | 10 Hz           | Sensor Fusion relies on the data to be updated at expected frequency for data synchronization.                      |                                                                                        |
| APER-015 | **Message rate** from CPP #5 to CPP #13              | Update rate of data sent from Camera-based Object detection.                                         | Object Recognition        | 10 Hz           | Sensor Fusion relies on the data to be updated at expected frequency for data synchronization.                      |                                                                                        |
| APER-016 | **Response time** from CPP #6 to CPP #13             | Response time to consume data sent from Clustering after LiDARs output pointcloud.                   | Object Recognition        | N/A             | Sensor Fusion relies on fresh and up-to-date data for data synchronization.                                         |                                                                                        |
| APER-017 | **Response time** from CPP #3 to CPP #13             | Response time to consume data sent from Camera-based Object detection after Cameras output images.   | Object Recognition        | N/A             | Sensor Fusion relies on fresh and up-to-date data for data synchronization.                                         |                                                                                        |
| APER-018 | **Message rate** from CPP #10 to CPP #17             | Update rate of data sent from Clustering.                                                            | Object Recognition        | 10 Hz           | Validator relies on the data to be updated at expected frequency for data synchronization.                          | It seems similar to APER-014, but the topic message is different.                      |
| APER-019 | **Message rate** from CPP #12 to CPP #17             | Update rate of data sent from DNN-based Object Recognition.                                          | Object Recognition        | 10 Hz           | Validator relies on the data to be updated at expected frequency for data synchronization.                          |
| APER-020 | **Response time** from CPP #6 to CPP #17 via CPP #10 | Response time to consume data sent from Clustering after LiDARs output pointcloud.                   | Object Recognition        | N/A             | Validator relies on fresh and update-date data for data synchronization.                                            | It seems similar to APER-015, but the topic message is different.                      |
| APER-021 | **Response time** from CPP #6 to CPP #17 via CPP #12 | Response time to consume data sent from DNN-based Object Recognition after LiDARs output pointcloud. | Object Recognition        | N/A             | Validator relies on fresh and update-date data for data synchronization.                                            |                                                                                        |

### Detailed temporal performance metrics for Paths between Obstacle segmentation and Planning

Obstacle segmentation, which is a crucial part of Perception, transmits data to Planning. The figure below illustrates the model that takes into account performance metrics related to Obstacle segmentation and Planning.

![Model for Obstacle segmentation temporal performance metrics](./images/important-temporal-performance-metrics/model-for-obstacle-segmentation-metrics.svg)

!!! note

    Both the Obstacle grid map and Obstacle segmentation transmit data to multiple sub-components of Planning. However, not all of these sub-components are described in the model. This is because our primary focus is on the paths from LiDAR to Planning via Obstacle segmentation.

The following list shows the temporal performance metrics around Obstacle segmentation and Planning.

| ID       | Representation in the model                          | Metric meaning                                                                           | Related functionality | Reference value | Reason to choose it as a metric                                                                                      | Note |
| -------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------- | --------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------- | ---- |
| OSEG-001 | **Message rate** from CPS #4 to CPS #7               | Update rate of Occupancy grid map received by Planning (`behavior_path_planner`)         | Obstacle segmentation | 10 Hz           | Planning relies on Occupancy grid map to be updated frequently and smoothly for creating accurate trajectory.        |      |
| OSEG-002 | **Response time** from CPS #0 to CPS #9 via CPS #7   | Response time to consume Occupancy grid map after LiDARs output sensing data.            | Obstacle segmentation | N/A             | Planning relies on fresh and up-to-date perceived data from Occupancy grid map for creating accurate trajectory..    |      |
| OSEG-003 | **Message rate** from CPS #6 to CPS #11              | Update rate of obstacle segmentation received by Planning (`behavior_velocity_planner`). | Obstacle segmentation | 10 Hz           | Planning relies on Obstacle segmentation to be updated frequently and smoothly for creating accurate trajectory.     |      |
| OSEG-004 | **Response time** from CPS #0 to CPS #13 via CPS #11 | Response time to consume Obstacle segmentation after LiDARs output sensing data.         | Obstacle segmentation | N/A             | Planning relies on fresh and up-to-date perceived data from Obstacle segmentation for creating accurate trajectory.. |      |
