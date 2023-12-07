利用可能なオープンソース SLAM
このページには、点群 (.pcd) マップ ファイルの生成に使用できる、利用可能なオープン ソースの Simultaneous Localization And Mapping (SLAM) 実装のリストが表示されます。

使用する実装の選択
Lidar オドメトリは時間の経過とともに累積的にドリフトします。この問題を解決するには、グラフの最適化、ループ クロージャ、GPS センサーを使用して累積ドリフト エラーを減らすなどのソリューションがあります。そのため、SLAM アルゴリズムにはループ クロージャ機能、グラフ最適化が必要であり、GPS センサーを使用する必要があります。さらに、一部のアルゴリズムは IMU センサーを使用してグラフに別の要素を追加し、ドリフト誤差を減少させています。アルゴリズムの中には厳密に 9 軸 IMU センサーを必要とするものもありますが、6 軸 IMU センサーのみを必要とするものや、IMU センサーを使用しないものもあります。Autoware 用のマップを作成するアルゴリズムを選択する前に、センサーの設定や生成されるマップの期待される品質に応じてこれらの要素が異なることを考慮してください。

チップ
一般的に使用されるオープンソース SLAM 実装は、lidarslam-ros2 (LiDAR、IMU*) およびLIO-SAM (LiDAR、IMU、GNSS) です。各アルゴリズムに必要なセンサー データは括弧内に指定されており、アスタリスク (*) はそのようなセンサー データがオプションであることを示します。サポートされている LiDAR モデルについては、各アルゴリズムの GitHub リポジトリを確認してください。これらの ROS 2 ベースの SLAM 実装は、Autoware を実行する同じマシンに簡単にインストールして直接使用できますが、ROS 1 ベースの代替案ほど十分にテストされておらず、成熟していない可能性があることに注意することが重要です。

ROS 1 に基づく注目すべきオープンソース SLAM 実装には、hdl-graph-slam (LiDAR、IMU*、GNSS*)、LeGO-LOAM (LiDAR、IMU*)、LeGO-LOAM-BOR (LiDAR)、LIOなどがあります。 -SAM (LiDAR、IMU、GNSS)。

これらのアルゴリズムのほとんどには、ループ クロージャとポーズ グラフの最適化がすでに組み込まれています。ただし、組み込みの自動ループ終了が失敗するか正しく動作しない場合は、インタラクティブ SLAMを使用してポーズ グラフを手動で調整および最適化できます。

サードパーティの SLAM 実装のリスト


パッケージ名	説明	リポジトリリンク	ループの閉鎖	センサー	ROSのバージョン	依存関係
ファスト-LIO-LC	ループ クロージャ モジュールとグラフ最適化を備えた、計算効率が高く堅牢な LiDAR 慣性オドメトリ パッケージ	https://github.com/yanliang-wang/FAST_LIO_LC	✓	Lidar
IMU
GPS [オプション]	ROS 1	ROS メロディック
PCL >= 1.8
固有 >= 3.3.4
GTSAM >= 4.0.0
FAST_LIO_SLAM	FAST_LIO_SLAM は、FAST_LIO と、スキャン コンテキスト ベースのループ検出および GTSAM ベースのポーズグラフ最適化である SC-PGO を統合したものです。	https://github.com/gisbi-kim/FAST_LIO_SLAM	✓	Lidar
IMU
GPS [オプション]	ROS 1	PCL >= 1.8
固有 >= 3.3.4
FD-SLAM	FD_SLAM は、Surface Representation Refinement に基づいた、Feature&Distribution ベースの 3D LiDAR SLAM メソッドです。このアルゴリズムでは、高速スキャン マッチングに新しい機能ベースの Lidar オドメトリが使用され、キーフレーム マッチングには提案された UGICP メソッドが使用されました。	https://github.com/SLAMWang/FD-SLAM	✓	Lidar
IMU [オプション]
GPS	ROS 1	PCL
g2o
スイートスパース
hdl_graph_slam	3D LIDAR を使用したリアルタイム 6DOF SLAM 用のオープンソース ROS パッケージ。これは、NDT スキャン マッチング ベースのオドメトリ推定とループ検出を備えた 3D Graph SLAM に基づいています。また、GPS、IMU 加速度 (重力ベクトル)、IMU 方向 (磁気センサー)、床面 (点群で検出) など、いくつかのグラフ制約もサポートします。	https://github.com/koide3/hdl_graph_slam	✓	Lidar
IMU [オプション]
GPS [オプション]	ROS 1	PCL
g2o
OpenMP
イア・リオ・サム	IA_LIO_SLAM は、非構造化環境でのデータ収集用に作成され、スムージングとマッピングを介した強度と周囲の強化されたライダー慣性走行距離測定のためのフレームワークであり、高精度のロボットの軌道とマッピングを実現します。	https://github.com/minwoo0611/IA_LIO_SAM	✓	ライダー
IMU
GPS	ROS 1	GTSAM
イスクローム	ISCLOAM は、ジオメトリ情報と強度情報の両方を統合することにより、堅牢なループ閉鎖検出アプローチを提供します。	https://github.com/wh200720041/iscloam	✓	ライダー	ROS 1	Ubuntu 18.04
ROS メロディック
セレス
PCL
GTSAM
OpenCV
レゴロームボール	LeGO-LOAM-BOR は、コードの品質を改善し、読みやすく一貫性を高めた LeGO-LOAM の改良版です。また、プロセスをマルチスレッドアプローチに変換することでパフォーマンスが向上します。	https://github.com/facontidavide/LeGO-LOAM-BOR	✓	ライダー
IMU	ROS 1	ROS メロディック
PCL
GTSAM
LIO_SAM	高精度かつリアルタイムな移動ロボットの軌道推定と地図構築を実現するフレームワーク。ファクター グラフの上に LIDAR 慣性オドメトリを定式化し、ループ クロージャを含む多数の相対および絶対測定をさまざまなソースからファクターとしてシステムに組み込むことができます。	https://github.com/TixiaoShan/LIO-SAM	✓	Lidar
IMU
GPS [オプション]	ROS 1
ROS 2	PCL
GTSAM
最適化されたSC-F-LOAM	F-LOAM の改良版であり、適応しきい値を使用してループ クロージャの検出結果をさらに判断し、誤ったループ クロージャの検出を減らします。また、特徴点ベースのマッチングを使用して、ループ クロージャ フレーム点群のペア間の制約を計算し、ループ フレーム制約の構築にかかる時間を削減します。	https://github.com/SlamCabbage/Optimized-SC-F-LOAM	✓	ライダー	ROS 1	PCL
GTSAM
セレス
スカローム	A-LOAM と ScanContext を統合したリアルタイム LiDAR SLAM パッケージ。	https://github.com/gisbi-kim/SC-A-LOAM	✓	ライダー	ROS 1	GTSAM >= 4.0
SC-レゴ-ローム	SC-LeGO-LOAM は、LIDAR オドメトリと 2 つの異なるループ クロージャ メソッド (ScanContext および Radius 検索ベースのループ クロージャ) 用に LeGO-LOAM を統合しました。ScanContext が大きなドリフトを修正している間、半径検索ベースの方法は細かいステッチングに適しています	https://github.com/irapkaist/SC-LeGO-LOAM	✓	ライダー
IMU	ROS 1	PCL
GTSAM
# Available Open Source SLAM

This page provides the list of available open source Simultaneous Localization And Mapping (SLAM) implementation that can be used to generate a point cloud (.pcd) map file.

## Selecting which implementation to use

Lidar odometry drifts accumulatively as time goes by and there is solutions to solve that problem such as graph optimization, loop closure and using gps sensor to decrease accumulative drift error. Because of that, a SLAM algorithm should have loop closure feature, graph optimization and should use gps sensor. Additionally, some of the algorithms are using IMU sensor to add another factor to graph for decreasing drift error. While some of the algorithms requires 9-axis IMU sensor strictly, some of them requires only 6-axis IMU sensor or not even using the IMU sensor. Before choosing an algorithm to create maps for Autoware please consider these factors depends on your sensor setup or expected quality of generated map.

## Tips

Commonly used open-source SLAM implementations are [lidarslam-ros2](https://github.com/rsasaki0109/lidarslam_ros2) (LiDAR, IMU\*) and [LIO-SAM](https://github.com/TixiaoShan/LIO-SAM/tree/ros2) (LiDAR, IMU, GNSS). The required sensor data for each algorithm is specified in the parentheses, where an asterisk (\*) indicates that such sensor data is optional. For supported LiDAR models, please check the GitHub repository of each algorithm. While these ROS 2-based SLAM implementations can be easily installed and used directly on the same machine that runs Autoware, it is important to note that they may not be as well-tested or as mature as ROS 1-based alternatives.

The notable open-source SLAM implementations that are based on ROS 1 include [hdl-graph-slam](https://github.com/koide3/hdl_graph_slam) (LiDAR, IMU\*, GNSS\*), [LeGO-LOAM](https://github.com/facontidavide/LeGO-LOAM-BOR) (LiDAR, IMU\*), [LeGO-LOAM-BOR](https://github.com/RobustFieldAutonomyLab/LeGO-LOAM) (LiDAR), and [LIO-SAM](https://github.com/TixiaoShan/LIO-SAM) (LiDAR, IMU, GNSS).

Most of these algorithms already have a built-in loop-closure and pose graph optimization. However, if the built-in, automatic loop-closure fails or does not work correctly, you can use [Interactive SLAM](https://github.com/SMRT-AIST/interactive_slam) to adjust and optimize a pose graph manually.

## List of Third Party SLAM Implementations

<!-- cspell: ignore UGICP Suitesparse -->

<br>
<br>

| Package Name        |                                                                                                                                                                         Explanation                                                                                                                                                                         |                                             Repository Link                                              | Loop Closure |                  Sensors                  |  ROS Version   |                          Dependencies                          |
| ------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------: | :----------: | :---------------------------------------: | :------------: | :------------------------------------------------------------: |
| FAST-LIO-LC         |                                                                                                                   A computationally efficient and robust LiDAR-inertial odometry package with loop closure module and graph optimization                                                                                                                    |       [https://github.com/yanliang-wang/FAST_LIO_LC](https://github.com/yanliang-wang/FAST_LIO_LC)       |   &check;    |      Lidar<br>IMU<br>GPS [Optional]       |     ROS 1      | ROS Melodic<br>PCL >= 1.8<br>Eigen >= 3.3.4<br>GTSAM >= 4.0.0  |
| FAST_LIO_SLAM       |                                                                                                         FAST_LIO_SLAM is the integration of FAST_LIO and SC-PGO which is scan context based loop detection and GTSAM based pose-graph optimization                                                                                                          |         [https://github.com/gisbi-kim/FAST_LIO_SLAM](https://github.com/gisbi-kim/FAST_LIO_SLAM)         |   &check;    |      Lidar<br>IMU<br>GPS [Optional]       |     ROS 1      |                  PCL >= 1.8<br>Eigen >= 3.3.4                  |
| FD-SLAM             |                                                       FD_SLAM is Feature&Distribution-based 3D LiDAR SLAM method based on Surface Representation Refinement. In this algorithm novel feature-based Lidar odometry used for fast scan-matching, and used a proposed UGICP method for keyframe matching                                                       |                [https://github.com/SLAMWang/FD-SLAM](https://github.com/SLAMWang/FD-SLAM)                |   &check;    |      Lidar<br>IMU [Optional]<br>GPS       |     ROS 1      |                   PCL<br>g2o<br>Suitesparse                    |
| hdl_graph_slam      |      An open source ROS package for real-time 6DOF SLAM using a 3D LIDAR. It is based on 3D Graph SLAM with NDT scan matching-based odometry estimation and loop detection. It also supports several graph constraints, such as GPS, IMU acceleration (gravity vector), IMU orientation (magnetic sensor), and floor plane (detected in a point cloud)      |           [https://github.com/koide3/hdl_graph_slam](https://github.com/koide3/hdl_graph_slam)           |   &check;    | Lidar<br>IMU [Optional]<br>GPS [Optional] |     ROS 1      |                      PCL<br>g2o<br>OpenMP                      |
| IA-LIO-SAM          |                                                       IA_LIO_SLAM is created for data acquisition in unstructured environment and it is a framework for Intensity and Ambient Enhanced Lidar Inertial Odometry via Smoothing and Mapping that achieves highly accurate robot trajectories and mapping                                                       |           [https://github.com/minwoo0611/IA_LIO_SAM](https://github.com/minwoo0611/IA_LIO_SAM)           |   &check;    |            Lidar<br>IMU<br>GPS            |     ROS 1      |                             GTSAM                              |
| ISCLOAM             |                                                                                                                      ISCLOAM presents a robust loop closure detection approach by integrating both geometry and intensity information                                                                                                                       |             [https://github.com/wh200720041/iscloam](https://github.com/wh200720041/iscloam)             |   &check;    |                   Lidar                   |     ROS 1      | Ubuntu 18.04<br>ROS Melodic<br>Ceres<br>PCL<br>GTSAM<br>OpenCV |
| LeGO-LOAM-BOR       |                                                                        LeGO-LOAM-BOR is improved version of the LeGO-LOAM by improving quality of the code, making it more readable and consistent. Also, performance is improved by converting processes to multi-threaded approach                                                                        |     [https://github.com/facontidavide/LeGO-LOAM-BOR](https://github.com/facontidavide/LeGO-LOAM-BOR)     |   &check;    |               Lidar<br>IMU                |     ROS 1      |                  ROS Melodic<br>PCL<br>GTSAM                   |
| LIO_SAM             |               A framework that achieves highly accurate, real-time mobile robot trajectory estimation and map-building. It formulates lidar-inertial odometry atop a factor graph, allowing a multitude of relative and absolute measurements, including loop closures, to be incorporated from different sources as factors into the system                |              [https://github.com/TixiaoShan/LIO-SAM](https://github.com/TixiaoShan/LIO-SAM)              |   &check;    |      Lidar<br>IMU<br>GPS [Optional]       | ROS 1<br>ROS 2 |                          PCL<br>GTSAM                          |
| Optimized-SC-F-LOAM | An improved version of F-LOAM and uses an adaptive threshold to further judge the loop closure detection results and reducing false loop closure detections. Also it uses feature point-based matching to calculate the constraints between a pair of loop closure frame point clouds and decreases time consumption of constructing loop frame constraints | [https://github.com/SlamCabbage/Optimized-SC-F-LOAM](https://github.com/SlamCabbage/Optimized-SC-F-LOAM) |   &check;    |                   Lidar                   |     ROS 1      |                     PCL<br>GTSAM<br>Ceres                      |
| SC-A-LOAM           |                                                                                                                                           A real-time LiDAR SLAM package that integrates A-LOAM and ScanContext.                                                                                                                                            |             [https://github.com/gisbi-kim/SC-A-LOAM](https://github.com/gisbi-kim/SC-A-LOAM)             |   &check;    |                   Lidar                   |     ROS 1      |                          GTSAM >= 4.0                          |
| SC-LeGO-LOAM        |                                                      SC-LeGO-LOAM integrated LeGO-LOAM for lidar odometry and 2 different loop closure methods: ScanContext and Radius search based loop closure. While ScanContext is correcting large drifts, radius search based method is good for fine-stitching                                                       |          [https://github.com/irapkaist/SC-LeGO-LOAM](https://github.com/irapkaist/SC-LeGO-LOAM)          |   &check;    |               Lidar<br>IMU                |     ROS 1      |                          PCL<br>GTSAM                          |
