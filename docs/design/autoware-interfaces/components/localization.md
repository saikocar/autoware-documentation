ローカリゼーション
ノード図

入力
点群マップ
点群で作成され、マップ サーバーによって公開された環境マップ。

センサー_msgs/msg/PointCloud2
3D 点群マップは、Autoware での LiDAR ベースのローカリゼーションに使用されます。

手動初期ポーズ
ユーザー インターフェイスによって公開される自我の開始ポーズ。

geometry_msgs/msg/PoseWithCovarianceStamped
std_msgs/msg/Headerヘッダー
geometry_msgs/msg/PoseWithCovariance ポーズ
geometry_msgs/msg/Pose ポーズ
geometry_msgs/msg/ポイント位置
geometry_msg/msg/クォータニオンの向き
double[36] 共分散
3D-LiDARスキャン
LiDAR センサーによって公開された、NDT マッチングのための LiDAR スキャン。

センサー_msgs/msg/PointCloud2
ローカリゼーションに使用する前に、生の 3D-LiDAR データを点群前処理モジュールで処理する必要があります。

自動初期ポーズ
INS(慣性航法センサー)のセンシングデータから計算した自我の開始姿勢。

geometry_msgs/msg/PoseWithCovarianceStamped
std_msgs/msg/Headerヘッダー
geometry_msgs/msg/PoseWithCovariance ポーズ
geometry_msgs/msg/Pose ポーズ
geometry_msgs/msg/ポイント位置
geometry_msg/msg/クォータニオンの向き
double[36] 共分散
初期ポーズが手動で設定されていない場合、メッセージを使用してポーズを自動初期化できます。

GNSS センサーによって公開された、自我の現在の地理座標。

センサー_msgs/msg/NavSatFix
std_msgs/msg/Headerヘッダー
sensor_msgs/msg/NavSatStatus ステータス
二重緯度
倍経度
二重高度
double[9] 位置共分散
ユニット 8 の位置共分散タイプ
GNSS-INS によって公開された自我の現在の方向。

autoware_sensing_msgs/msg/GnssInsOrientationStamped
std_msgs/Header ヘッダー
autoware_sensing_msgs/msg/GnssInsOrientation の方向
geometry_msgs/クォータニオンの向き
float32 rmse_rotation_x
float32 rmse_rotation_y
float32 rmse_rotation_z
IMUデータ
IMU センシング データから計算された自我の現在の向き、角速度、直線加速度。

センサー_msgs/msg/Imu
std_msgs/msg/Headerヘッダー
geometry_msgs/msg/クォータニオンの向き
double[9] 方向共分散
geometry_msgs/msg/Vector3 angular_velocity
double[9] angular_velocity_covariance
geometry_msgs/msg/Vector3 線形加速
double[9] 線形加速度共分散
車速ステータス
自車両の現在の速度。車両インターフェースによって公開されます。

autoware_auto_vehicle_msgs/msg/VelocityReport
std_msgs/msg/Header ヘッダー。
浮動小数点縦速度;
float 横速度;
浮動小数点見出し率;
速度入力ローカリゼーション インターフェイスの前に、モジュールはvehicle_velocity_converterメッセージ タイプautoware_auto_vehicle_msgs/msg/VelocityReportを に変換しますgeometry_msgs/msg/TwistWithCovarianceStamped。

出力
乗り物のポーズ
ローカリゼーション インターフェイスから計算された自我の現在の姿勢。

geometry_msgs/msg/PoseWithCovarianceStamped
std_msgs/msg/Headerヘッダー
geometry_msg/PoseWithCovariance ポーズ
geometry_msgs/msg/Pose ポーズ
geometry_msgs/msg/ポイント位置
geometry_msgs/msg/クォータニオンの向き
double[36] 共分散
車速
自我の現在の速度。ローカリゼーション インターフェイスから計算されます。

geometry_msgs/msg/TwistWithCovarianceStamped
std_msgs/msg/Headerヘッダー
geometry_msg/TwistWithCovarianceツイスト
geometry_msgs/msg/ツイストツイスト
geometry_msgs/msg/Vector3 線形
geometry_msgs/msg/Vector3 角度
double[36] 共分散
車両の加速度
自我の現在の加速度。ローカリゼーション インターフェイスから計算されます。

geometry_msgs/msg/AccelWithCovarianceStamped
std_msgs/msg/Headerヘッダー
geometry_msg/AccelWithCovariance アクセル
geometry_msgs/msg/Accel アクセル
geometry_msgs/msg/Vector3 線形
geometry_msgs/msg/Vector3 角度
double[36] 共分散
車両の運動学的状態
自我の現在の姿勢、速度、加速度。ローカリゼーション インターフェイスから計算されます。

注:運動学的状態には、姿勢、速度、加速度が含まれます。将来的には、ポーズ、速度、加速度はローカリゼーションの出力として使用されなくなります。

autoware_msgs/autoware_localization_msgs/msg/KinematicState
std_msgs/msg/Headerヘッダー
文字列 child_frame_id
geometry_msgs/PoseWithCovariancepose_with_covariance
geometry_msgs/TwistWithCovariance ツイストウィズ共分散
geometry_msgs/AccelWithCovariance accel_with_covariance
メッセージは計画および制御モジュールによってサブスクライブされます。

位置特定の精度
ローカリゼーション モジュールが適切に動作しているかどうかを示す診断情報。

未定。
# Localization

![Node diagram](images/Localization-Bus-ODD-Architecture.drawio.svg)

## Inputs

### Pointcloud Map

Environment map created with point cloud, published by the map server.

- sensor_msgs/msg/PointCloud2

A 3d point cloud map is used for LiDAR-based localization in Autoware.

### Manual Initial Pose

Start pose of ego, published by the user interface.

- geometry_msgs/msg/PoseWithCovarianceStamped
  - std_msgs/msg/Header header
  - geometry_msgs/msg/PoseWithCovariance pose
    - geometry_msgs/msg/Pose pose
      - geometry_msgs/msg/Point position
      - geometry_msg/msg/Quaternion orientation
    - double[36] covariance

### 3D-LiDAR Scanning

LiDAR scanning for NDT matching, published by the LiDAR sensor.

- sensor_msgs/msg/PointCloud2

The raw 3D-LiDAR data needs to be processed by the [point cloud pre-processing modules](../../autoware-architecture/sensing/data-types/point-cloud.md) before being used for localization.

### Automatic Initial pose

Start pose of ego, calculated from INS(Inertial navigation sensor) sensing data.

- geometry_msgs/msg/PoseWithCovarianceStamped
  - std_msgs/msg/Header header
  - geometry_msgs/msg/PoseWithCovariance pose
    - geometry_msgs/msg/Pose pose
      - geometry_msgs/msg/Point position
      - geometry_msg/msg/Quaternion orientation
    - double[36] covariance

When the initial pose is not set manually, the message can be used for automatic pose initialization.

Current Geographic coordinate of the ego, published by the GNSS sensor.

- sensor_msgs/msg/NavSatFix
  - std_msgs/msg/Header header
  - sensor_msgs/msg/NavSatStatus status
  - double latitude
  - double longitude
  - double altitude
  - double[9] position_covariance
  - unit8 position_covariance_type

Current orientation of the ego, published by the GNSS-INS.

- autoware_sensing_msgs/msg/GnssInsOrientationStamped
  - std_msgs/Header header
  - autoware_sensing_msgs/msg/GnssInsOrientation orientation
    - geometry_msgs/Quaternion orientation
    - float32 rmse_rotation_x
    - float32 rmse_rotation_y
    - float32 rmse_rotation_z

### IMU Data

Current orientation, angular velocity and linear acceleration of ego, calculated from IMU sensing data.

- sensor_msgs/msg/Imu
  - std_msgs/msg/Header header
  - geometry_msgs/msg/Quaternion orientation
  - double[9] orientation_covariance
  - geometry_msgs/msg/Vector3 angular_velocity
  - double[9] angular_velocity_covariance
  - geometry_msgs/msg/Vector3 linear_acceleration
  - double[9] linear_acceleration_covariance

### Vehicle Velocity Status

Current velocity of the ego vehicle, published by the vehicle interface.

- autoware_auto_vehicle_msgs/msg/VelocityReport
  - std_msgs/msg/Header header;
  - float longitudinal_velocity;
  - float lateral_velocity;
  - float heading_rate;

Before the velocity input localization interface, module `vehicle_velocity_converter` converts message type `autoware_auto_vehicle_msgs/msg/VelocityReport` to `geometry_msgs/msg/TwistWithCovarianceStamped`.

## Outputs

### Vehicle pose

Current pose of ego, calculated from localization interface.

- geometry_msgs/msg/PoseWithCovarianceStamped
  - std_msgs/msg/Header header
  - geometry_msg/PoseWithCovariance pose
    - geometry_msgs/msg/Pose pose
      - geometry_msgs/msg/Point position
      - geometry_msgs/msg/Quaternion orientation
    - double[36] covariance

### Vehicle velocity

Current velocity of ego, calculated from localization interface.

- geometry_msgs/msg/TwistWithCovarianceStamped
  - std_msgs/msg/Header header
  - geometry_msg/TwistWithCovariance twist
    - geometry_msgs/msg/Twist twist
      - geometry_msgs/msg/Vector3 linear
      - geometry_msgs/msg/Vector3 angular
    - double[36] covariance

### Vehicle acceleration

Current acceleration of ego, calculated from localization interface.

- geometry_msgs/msg/AccelWithCovarianceStamped
  - std_msgs/msg/Header header
  - geometry_msg/AccelWithCovariance accel
    - geometry_msgs/msg/Accel accel
      - geometry_msgs/msg/Vector3 linear
      - geometry_msgs/msg/Vector3 angular
    - double[36] covariance

### Vehicle kinematic state

Current pose, velocity and acceleration of ego, calculated from localization interface.

**Note:** Kinematic state contains pose, velocity and acceleration. In the future, [pose](#vehicle-pose), [velocity](#vehicle-velocity) and [acceleration](#vehicle-acceleration) will not be used as output for localization.

- autoware_msgs/autoware_localization_msgs/msg/KinematicState
  - std_msgs/msg/Header header
  - string child_frame_id
  - geometry_msgs/PoseWithCovariance pose_with_covariance
  - geometry_msgs/TwistWithCovariance twist_with_covariance
  - geometry_msgs/AccelWithCovariance accel_with_covariance

The message will be subscribed by the planning and control module.

### Localization Accuracy

Diagnostics information that indicates if the localization module works properly.

TBD.
