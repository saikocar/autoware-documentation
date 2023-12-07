ローカリゼーションコンポーネントの設計ドキュメント

抽象的な
1. 要件
位置特定の目的は、車両の姿勢、速度、加速度を推定することです。

目標:

車両の姿勢、速度、加速度を可能な限り長く推定できるシステムを提案します。
推定の安定性を診断し、推定結果が信頼できない場合にはエラー監視システムに警告メッセージを送信できるシステムを提案します。
さまざまなセンサー構成と連携できる車両位置特定機能を設計します。
目標以外:

この設計文書は、次のようなローカリゼーション システムを開発することを目的としたものではありません。
あらゆる環境において確実である
事前に定義された ODD (運用設計ドメイン) の外側で機能する
自動運転に必要な性能より優れている
2. センサー構成例
このセクションでは、センサー構成の例とその予想されるパフォーマンスを示します。各センサーには独自の長所と短所がありますが、複数のセンサーを融合することで全体的なパフォーマンスを向上させることができます。

3D-LiDAR + 点群マップ
予想される状況
車両が都市部などの構造物が多い環境に設置されている
システムが不安定になる可能性がある状況
車両が田園風景、高速道路、トンネルなどの構造物のない環境に置かれている
マップ作成時と比べて、積雪や建物の建設・破壊などの環境変化が発生しています。
周囲のオブジェクトが遮られる
車の周囲が、ガラス窓、反射、吸収（暗い物体）など、LiDAR で検出できない物体に囲まれている
環境には、車の LiDAR センサーと同じ周波数のレーザー ビームが含まれています。
機能性
このシステムは、点群マップ上の車両の位置を最大 10 cm の誤差で推定できます。
システムは夜間でも動作可能です。
3D-LiDAR またはカメラ + ベクトルマップ
予想される状況
高速道路や一般道路など、白線がはっきりしていて曲率が緩やかな道路。
システムが不安定になる可能性がある状況
白い線が擦れたり、雨や雪で隠れたりする
交差点などの急な曲率
雨や塗装による路面の反射変化が大きい
機能
横方向の車両位置を修正します。
経度方向に沿った姿勢補正は不正確になる可能性がありますが、GNSS と融合することで解決できます。
GNSS
予想される状況
車両は、田園風景など、周囲に物がほとんど、またはまったくないオープンな環境に配置されます。
システムが不安定になる可能性がある状況
GNSS 信号は、トンネルや建物などの周囲の物体によってブロックされます。
機能性
このシステムは、世界座標における車両の位置を最大 10 メートルの誤差以内で推定できます。
RKT-GNSS (リアルタイム運動学全地球航法衛星システム) を取り付けると、精度は最大 10cm まで向上します。
この構成のシステムは、環境マップ (点群マップとベクトル マップ タイプの両方) なしで動作できます。
カメラ（ビジュアルオドメトリ、ビジュアルSLAM）
予想される状況
車両は市街地などの視覚的特徴が豊かな環境に設置されます。
システムが不安定になる可能性がある状況
車両はテクスチャのない環境に置かれます。
車両は他の物体に囲まれています。
カメラは、太陽光、他の車両のヘッドライト、またはトンネルの出口に近づいたときなどに生じる重大な照明の変化を観察します。
車両は暗い環境に置かれます。
機能性
このシステムは、視覚的特徴を追跡することでオドメトリを推定できます。
車輪速センサー
予想される状況
車両は平坦で滑らかな道路を走行しています。
システムが不安定になる可能性がある状況
車両が滑りやすい路面やでこぼこした路面を走行しているため、車輪速度が不正確に測定される可能性があります。
機能性
車速を取得し、走行距離を推定することができます。
IMU
想定される環境
平らで滑らかな道路
システムが不安定になる可能性がある状況
IMU には周囲の温度に依存するバイアス1があり、不正確なセンサー観測やオドメトリ ドリフトを引き起こす可能性があります。
機能性
このシステムは加速度と角速度を観測できます。
これらの観測を統合することで、システムは局所的な姿勢変化を推定し、推測航法を実現できます。
地磁気センサー
予想される状況
車両が磁気ノイズの少ない環境に置かれている
システムが不安定になる可能性がある状況
車両が、電磁波を発生する鉄筋やその他の材料を使用した建物や構造物など、磁気ノイズの高い環境に置かれている。
機能性
このシステムは、世界座標系で車両の方向を推定できます。
磁気マーカー
予想される状況
自動車は磁気マーカーが設置された環境に置かれます。
システムが不安定になる状況
マーカーは維持されません。
機能性
磁気マーカを検出することで、世界座標上での車両位置を取得できます。
道路が雪で覆われている場合でもシステムは機能します。
3. 要件
さまざまなモジュールを実装することで、さまざまなセンサー構成とアルゴリズムを使用できます。
位置特定システムは、あいまいな初期位置から姿勢推定を開始できます。
このシステムは、信頼性の高い初期位置推定を生成できます。
システムは、初期位置推定の状態 (初期化されていない、初期化可能、または初期化不可能) を管理し、エラー モニターに報告できます。
4. アーキテクチャ
抽象的な
「必須」と「推奨」の 2 つのアーキテクチャが定義されています。ただし、「必須」アーキテクチャには、さまざまなローカリゼーション アルゴリズムを受け入れるために必要な入力と出力のみが含まれています。各モジュールの再利用性を向上させるために、必要なコンポーネントは、より詳細な説明とともに「推奨」アーキテクチャのセクションで定義されています。

必要なアーキテクチャ
必要なアーキテクチャ

入力
センサーメッセージ
例: LiDAR、カメラ、GNSS、IMU、CAN バスなど。
再利用可能にするために、データ型は ROS プリミティブである必要があります
地図データ
例: 点群マップ、lanlet2 マップ、フィーチャ マップなど。
マップ形式はユースケースとセンサー構成に基づいて選択する必要があります
一部の特定のケース (GNSS のみの位置特定など) では地図データが必要ないことに注意してください。
tf、static_tf
マップフレーム
ベースリンクフレーム
出力
共分散スタンプ付きのポーズ
マップ座標上の車両の姿勢、共分散、およびタイムスタンプ
50Hz~ 周波数 (計画および制御コンポーネントの要件による)
共分散スタンプ付きツイスト
Base_link 座標の車両速度、共分散、およびタイムスタンプ
50Hz～周波数
共分散スタンプ付きのアクセル
Base_link 座標の加速度、共分散、およびタイムスタンプ
50Hz～周波数
診断
ローカリゼーション モジュールが適切に動作しているかどうかを示す診断情報
TF
Base_link へのマップの tf
推奨されるアーキテクチャ
推奨アーキテクチャ

姿勢推定器
外部センサーの観測値と地図を照合することで、地図座標上の車両の姿勢を推定します
取得したポーズとその共分散を提供します。PoseTwistFusionFilter
ツイスト-アクセル推定器
車両速度、角速度、加速度、角加速度、およびそれらの共分散を生成します。
ツイストとアクセラレーションの両方に対して 1 つのモジュールを作成することも、2 つの別個のモジュールを作成することも可能です。アーキテクチャの選択は開発者次第です。
ねじれ推定器は、内部センサーの観察から速度と角速度を生成します。
加速度推定器は、内部センサーの観測値から加速度と角加速度を生成します。
キネマティクス フュージョン フィルター
2 種類の情報を融合して計算された、最も可能性の高い姿勢、速度、加速度、およびそれらの共分散を生成します。
姿勢推定器から取得された姿勢。
ツイスト加速推定器から得られる速度と加速度
姿勢推定結果に従ってbase_linkへのマップのtfを生成します
ローカリゼーション診断
複数の位置推定モジュールから取得した情報を融合することにより、姿勢推定の安定性と信頼性を監視および保証します
エラーステータスをエラーモニターに報告します
TFツリー
TFツリー

フレーム	意味
地球	ECEF（地球中心固定）
地図	地図座標の原点（例：MGRS原点）
ビューア	rviz のユーザー定義フレーム
ベースリンク	自車両の基準姿勢（後軸中心の地面への投影）
センサー	各センサーの基準姿勢
開発者は、上記の tf 構造が維持されている限り、オプションで odom やbase_footprint などの他のフレームを追加できます。

ローカリゼーションモジュールの理想的な機能
位置特定モジュールは、制御、計画、知覚のために姿勢、速度、加速度を提供する必要があります。
レイテンシーとスタガーは、推定値を ODD (運用設計ドメイン) 内の制御に使用できるように、十分に小さいか調整できる必要があります。
位置特定モジュールは、固定座標フレーム上にポーズを生成する必要があります。
センサーは簡単に交換できるように、互いに独立している必要があります。
位置特定モジュールは、自律走行車が自己完結型機能または地図情報で動作できるかどうかを示すステータスを提供する必要があります。
ツールまたはマニュアルには、ローカリゼーション モジュールの適切なパラメータを設定する方法が記載されている必要があります。
さまざまなフレームまたはポーズの座標とセンサーのタイムスタンプを調整するには、有効なキャリブレーション パラメーターを指定する必要があります。
KPI
安全な操作のために十分な姿勢推定パフォーマンスを維持するには、次の指標が考慮されます。

安全性
姿勢推定が必要な精度を満たした ODD 内での移動距離を、ODD 内で移動した全体の距離で割った値 (パーセンテージ)。
位置特定モジュールが ODD 内の姿勢を推定できない状況の異常検出率
車両が ODD の外側に出たときの検出精度 (パーセンテージ)。
計算負荷
レイテンシ
5. インターフェースとデータ構造
6. 懸念事項、仮定、制限事項
センサーと入力の前提条件
センサーの前提条件
入力データに異常はありません。
IMUなどの内部センサー観測により適切な周波数を継続的に維持します。
入力データには正確なタイムスタンプが付いています。
タイムスタンプが正確でない場合、推定されたポーズが不正確になったり、不安定になる可能性があります。
センサーは正確な位置に正しく取り付けられており、TF からアクセスできます。
センサーの位置が不正確な場合、推定結果が正しくなかったり、不安定になる可能性があります。
センサーの位置を適切に取得するには、センサー キャリブレーション フレームワークが必要です。
マップの前提条件
地図内には十分な情報が含まれています。
マップ内の情報が不十分な場合、姿勢推定が不安定になる可能性があります。
マップに姿勢推定に十分な情報があるかどうかを確認するには、テスト フレームワークが必要です。
マップは実際の環境と大きく変わりません。
実際の環境にマップとは異なるオブジェクトがある場合、姿勢推定が不安定になる可能性があります。
地図は、新しいオブジェクトや季節の変化に応じて更新する必要があります。
マップは均一の座標に揃える必要があります。そうでない場合は、調整フレームワークが導入されている必要があります。
異なる座標系を持つ複数のマップが使用される場合、それらの間の位置ずれが位置特定のパフォーマンスに影響を与える可能性があります。
計算リソース
精度と計算速度を維持するには、十分な計算リソースを提供する必要があります。
脚注
バイアスの詳細については、VectorNav IMU 仕様ページを参照してください。↩
LOCALIZATION COMPONENT DESIGN DOC

## Abstract

## 1. Requirements

Localization aims to estimate vehicle pose, velocity, and acceleration.

Goals:

- Propose a system that can estimate vehicle pose, velocity, and acceleration for as long as possible.
- Propose a system that can diagnose the stability of estimation and send a warning message to the error-monitoring system if the estimation result is unreliable.
- Design a vehicle localization function that can work with various sensor configurations.

Non-goals:

- This design document does not aim to develop a localization system that
  - is infallible in all environments
  - works outside of the pre-defined ODD (Operational Design Domain)
  - has better performance than is required for autonomous driving

## 2. Sensor Configuration Examples

This section shows example sensor configurations and their expected performances.
Each sensor has its own advantages and disadvantages, but overall performance can be improved by fusing multiple sensors.

### 3D-LiDAR + PointCloud Map

#### Expected situation

- The vehicle is located in a structure-rich environment, such as an urban area

#### Situations that can make the system unstable

- The vehicle is placed in a structure-less environment, such as a rural landscape, highway, or tunnel
- Environmental changes have occurred since the map was created, such as snow cover or the construction/destruction of buildings.
- Surrounding objects are occluded
- The car is surrounded by objects undetectable by LiDAR, e.g., glass windows, reflections, or absorption (dark objects)
- The environment contains laser beams at the same frequency as the car's LiDAR sensor(s)

#### Functionality

- The system can estimate the vehicle location on the point cloud map with the error of ~10cm.
- The system is operable at night.

### 3D-LiDAR or Camera + Vector Map

#### Expected situation

- Road with clear white lines and loose curvatures, such as a highway or an ordinary local road.

#### Situations that can make the system unstable

- White lines are scratchy or covered by rain or snow
- Tight curvature such as intersections
- Large reflection change of the road surface caused by rain or paint

#### Functionalities

- Correct vehicle positions along the lateral direction.
- Pose correction along the longitudinal can be inaccurate, but can be resolved by fusing with GNSS.

### GNSS

#### Expected situation

- The vehicle is placed in an open environment with few to no surrounding objects, such as a rural landscape.

#### Situation that can make the system unstable

- GNSS signals are blocked by surrounding objects, e.g., tunnels or buildings.

#### Functionality

- The system can estimate vehicle position in the world coordinate within an error of ~10m.
- With a RKT-GNSS (Real Time Kinematic Global Navigation Satellite System) attached, the accuracy can be improved to ~10cm.
- A system with this configuration can work without environment maps (both point cloud and vector map types).

### Camera (Visual Odometry, Visual SLAM)

#### Expected situation

- The vehicle is placed in an environment with rich visual features, such as an urban area.

#### Situations that can make the system unstable

- The vehicle is placed in a texture-less environment.
- The vehicle is surrounded by other objects.
- The camera observes significant illumination changes, such as those caused by sunshine, headlights from other vehicles or when approaching the exit of a tunnel.
- The vehicle is placed in a dark environment.

#### Functionality

- The system can estimate odometry by tracking visual features.

### Wheel speed sensor

#### Expected situation

- The vehicle is running on a flat and smooth road.

#### Situations that can make the system unstable

- The vehicle is running on a slippery or bumpy road, which can cause incorrect observations of wheel speed.

#### Functionality

- The system can acquire the vehicle velocity and estimate distance traveled.

### IMU

#### Expected environments

- Flat, smooth roads

#### Situations that can make the system unstable

- IMUs have a bias[^1] that is dependent on the surrounding temperature, and can cause incorrect sensor observation or odometry drift.

[^1]: For more details about bias, refer to the [VectorNav IMU specifications page](https://www.vectornav.com/resources/inertial-navigation-primer/specifications--and--error-budgets/specs-imuspecs).

#### Functionality

- The system can observe acceleration and angular velocity.
- By integrating these observations, the system can estimate the local pose change and realize dead-reckoning

### Geomagnetic sensor

#### Expected situation

- The vehicle is placed in an environment with low magnetic noise

#### Situations that can make the system unstable

- The vehicle is placed in an environment with high magnetic noise, such as one containing buildings or structures with reinforced steel or other materials that generate electromagnetic waves.

#### Functionality

- The system can estimate the vehicle's direction in the world coordinate system.

### Magnetic markers

#### Expected situation

- The car is placed in an environment with magnetic markers installed.

#### Situations where the system becomes unstable

- The markers are not maintained.

#### Functionality

- Vehicle location can be obtained on the world coordinate by detecting the magnetic markers.
- The system can work even if the road is covered with snow.

## 3. Requirements

- By implementing different modules, various sensor configurations and algorithms can be used.
- The localization system can start pose estimation from an ambiguous initial location.
- The system can produce a reliable initial location estimation.
- The system can manage the state of the initial location estimation (uninitialized, initializable, or non-initializable) and can report to the error monitor.

## 4. Architecture

### Abstract

Two architectures are defined, "Required" and "Recommended". However, the "Required" architecture only contains the inputs and outputs necessary to accept various localization algorithms. To improve the reusability of each module, the required components are defined in the "Recommended" architecture section along with a more detailed explanation.

### Required Architecture

![required-architecture](../image/localization/required-architecture.png)

#### Input

- Sensor message
  - e.g., LiDAR, camera, GNSS, IMU, CAN Bus, etc.
  - Data types should be ROS primitives for reusability
- Map data
  - e.g., point cloud map, lanelet2 map, feature map, etc.
  - The map format should be chosen based on use case and sensor configuration
  - Note that map data is not required for some specific cases (e.g., GNSS-only localization)
- tf, static_tf
  - map frame
  - base_link frame

#### Output

- Pose with covariance stamped
  - Vehicle pose, covariance, and timestamp on the map coordinate
  - 50Hz~ frequency (depending on the requirements of the Planning and Control components)
- Twist with covariance stamped
  - Vehicle velocity, covariance, and timestamp on the base_link coordinate
  - 50Hz~ frequency
- Accel with covariance stamped
  - Acceleration, covariance, and timestamp on the base_link coordinate
  - 50Hz~ frequency
- Diagnostics
  - Diagnostics information that indicates if the localization module works properly
- tf
  - tf of map to base_link

### Recommended Architecture

![recommended-architecture](../image/localization/recommended-architecture.png)

#### Pose Estimator

- Estimates the vehicle pose on the map coordinate by matching external sensor observation to the map
- Provides the obtained pose and its covariance to `PoseTwistFusionFilter`

#### Twist-Accel Estimator

- Produces the vehicle velocity, angular velocity, acceleration, angular acceleration, and their covariances
  - It is possible to create a single module for both twist and acceleration or to create two separate modules - the choice of architecture is up to the developer
- The twist estimator produces velocity and angular velocity from internal sensor observation
- The accel estimator produces acceleration and angular acceleration from internal sensor observations

#### Kinematics Fusion Filter

- Produces the likeliest pose, velocity, acceleration, and their covariances, computed by fusing two kinds of information:
  - The pose obtained from the pose estimator.
  - The velocity and acceleration obtained from the twist-accel estimator
- Produces tf of map to base_link according to the pose estimation result

#### Localization Diagnostics

- Monitors and guarantees the stability and reliability of pose estimation by fusing information obtained from multiple localization modules
- Reports error status to the error monitor

#### TF tree

![tf-tree](../image/localization/tf-tree.png)

|   frame   | meaning                                                                                        |
| :-------: | :--------------------------------------------------------------------------------------------- |
|   earth   | ECEF (Earth Centered Earth Fixed）                                                             |
|    map    | Origin of the map coordinate (ex. MGRS origin)                                                 |
|  viewer   | User-defined frame for rviz                                                                    |
| base_link | Reference pose of the ego-vehicle (projection of the rear-axle center onto the ground surface) |
|  sensor   | Reference pose of each sensor                                                                  |

Developers can optionally add other frames such as odom or base_footprint as long as the tf structure above is maintained.

### The localization module's ideal functionality

- The localization module should provide pose, velocity, and acceleration for control, planning, and perception.
- Latency and stagger should be sufficiently small or adjustable such that the estimated values can be used for control within the ODD (Operational Design Domain).
- The localization module should produce the pose on a fixed coordinate frame.
- Sensors should be independent of each other so that they can be easily replaced.
- The localization module should provide a status indicating whether or not the autonomous vehicle can operate with the self-contained function or map information.
- Tools or manuals should describe how to set proper parameters for the localization module
- Valid calibration parameters should be provided to align different frame or pose coordinates and sensor timestamps.

### KPI

To maintain sufficient pose estimation performance for safe operation, the following metrics are considered:

- Safety
  - The distance traveled within the ODD where pose estimation met the required accuracy, divided by the overall distance traveled within the ODD, as a percentage.
  - The anomaly detection rate for situations where the localization module cannot estimate pose within the ODD
  - The accuracy of detecting when the vehicle goes outside of the ODD, as a percentage.
- Computational load
- Latency

## 5. Interface and Data Structure

## 6. Concerns, Assumptions, and Limitations

### Prerequisites of sensors and inputs

#### Sensor prerequisites

- Input data is not defective.
  - Internal sensor observation such as IMU continuously keeps the proper frequency.
- Input data has correct and exact time stamps.
  - Estimated poses can be inaccurate or unstable if the timestamps are not exact.
- Sensors are correctly mounted with exact positioning and accessible from TF.
  - If the sensor positions are inaccurate, estimation results may be incorrect or unstable.
  - A sensor calibration framework is required to properly obtain the sensor positions.

#### Map prerequisites

- Sufficient information is contained within the map.
  - Pose estimation might be unstable if there is insufficient information in the map.
  - A testing framework is necessary to check if the map has adequate information for pose estimation.
- Map does not differ greatly from the actual environment.
  - Pose estimation might be unstable if the actual environment has different objects from the map.
  - Maps need updates according to new objects and seasonal changes.
- Maps must be aligned to a uniform coordinate, or an alignment framework is in place.
  - If multiple maps with different coordinate systems are used, the misalignment between them can affect the localization performance.

#### Computational resources

- Sufficient computational resources should be provided to maintain accuracy and computation speed.
