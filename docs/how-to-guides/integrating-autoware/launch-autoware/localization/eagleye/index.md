# AutowareでEagleyeを使用する

このページでは、 Autoware で使用するために[Eagleye](https://github.com/MapIV/eagleye)をセットアップする方法を説明します。
統合提案の詳細については、[こちらのディスカッション](https://github.com/orgs/autowarefoundation/discussions/3257)を参照してください。

## Eagleyeとは何か?

Eagleye は、[MAP IV. Inc](https://map4.jp/)によって最初に開発されたオープンソースの GNSS/IMU ベースのローカライザーです。低コストの GNSS センサーと IMU センサーを使用して車両の位置、方位、および高度の情報を提供することで、LiDAR および点群ベースの位置特定に代わる費用対効果の高い代替手段を提供します。

### 依存関係

以下のパッケージは[autoware.repos](https://github.com/autowarefoundation/autoware/blob/main/autoware.repos)にリストされているため、Autoware のセットアップ中に自動的にインストールされます。

1. [Eagleye](https://github.com/MapIV/eagleye.git) (autoware-main branch)
2. [RTKLIB ROS Bridge](https://github.com/MapIV/rtklib_ros_bridge.git) (ros2-v0.1.0 branch)
3. [LLH Converter](https://github.com/MapIV/llh_converter.git) (ros2 branch)

## アーキテクチャ

Eagleye は、Autoware ローカリゼーション スタックで次の 2 つの方法で利用できます:

1. EKF ローカライザーにツイストのみをフィードします。

   ![Eagleye ツイストの統合](images/eagleye-integration-guide/eagleye_twist.drawio.svg)

2. Eagleye からツイストとポーズの両方を EKF ローカライザーにフィードします (ツイストは通常​​の`gyro_odometry`で使用することもできます)。

   ![Eagleye ポーズ ツイストの統合](images/eagleye-integration-guide/eagleye_pose_twist.drawio.svg)

**注記: Eagleye を姿勢推定器として使用する場合は、RTK 位置決めが必要です。**
一方、ツイスト推定器として使用する場合は必須ではありません。

## 要件

Eagleye では、入力として GNSS、IMU、車両速度が必要です。

### IMUトピック

`sensor_msgs/msg/Imu`はEagleye IMU 入力でサポートされています。

### 車速トピック

`geometry_msgs/msg/TwistStamped`および`geometry_msgs/msg/TwistWithCovarianceStamped`は入力車両速度に対してサポートされます。

### GNSSトピック

Eagleye には、GNSS 受信機によって生成された緯度/経度の高さとドップラー速度が必要です。
GNSS ROS ドライバーは次のメッセージを発行する必要があります:

- `sensor_msgs/msg/NavSatFix`: このメッセージには、緯度、経度、高さの情報が含まれています。
- `geometry_msgs/msg/TwistWithCovarianceStamped`: このメッセージには、GNSS ドップラー速度情報が含まれています。
  <!-- cspell: ignore ublox -->
  Eagleye は、GNSS ROS ドライバーの次の例を使用してテストされています。: ublox_gpsおよびseptentrio_gnss_driver. これらの各ドライバーに必要な設定は次のとおりです:

| GNSS ROSドライバー                                                                              | 修正                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [ublox_gps](https://github.com/KumarRobotics/ublox/tree/ros2/ublox_gps)                       | 追加の設定は必要ありません。これは`sensor_msgs/msg/NavSatFix`と`geometry_msgs/msg/TwistWithCovarianceStamped`がデフォルト設定で公開され、Eagleyeによって要求されます。      |
| [septentrio_gnss_driver](https://github.com/septentrio-gnss/septentrio_gnss_driver/tree/ros2) | [`gnss.yaml`](https://github.com/septentrio-gnss/septentrio_gnss_driver/blob/ros2/config/gnss.yaml#L90)構成ファイルで`publish.navsatfix`と`publish.twist`を`true` に設定します |

## 車両に組み込むためのパラメータ変更

### トピック名とトピックの型

ユーザーは、 で GNSS 緯度、経度、高さ 、GNSS ドップラー速度 、IMU 、および車両速度の入力トピックを[`eagleye_config.yaml`](https://github.com/MapIV/autoware_launch/blob/3f04a9dd7bc4a4c49d4ec790e3f6b9958ab822da/autoware_launch/config/localization/eagleye_config.param.yaml#L7-L16)で正しく指定する必要があります。

```yaml
# Topic
twist:
  twist_type: 1 # TwistStamped : 0, TwistWithCovarianceStamped: 1
  twist_topic: /sensing/vehicle_velocity_converter/twist_with_covariance
imu_topic: /sensing/imu/tamagawa/imu_raw
gnss:
  velocity_source_type: 2 # rtklib_msgs/RtklibNav: 0, nmea_msgs/Sentence: 1, ublox_msgs/NavPVT: 2, geometry_msgs/TwistWithCovarianceStamped: 3
  velocity_source_topic: /sensing/gnss/ublox/navpvt
  llh_source_type: 2 # rtklib_msgs/RtklibNav: 0, nmea_msgs/Sentence: 1, sensor_msgs/NavSatFix: 2
  llh_source_topic: /sensing/gnss/ublox/nav_sat_fix
```

### センサーの周波数

また、 [`eagleye_config.yaml`](https://github.com/MapIV/autoware_launch/blob/3f04a9dd7bc4a4c49d4ec790e3f6b9958ab822da/autoware_launch/config/localization/eagleye_config.param.yaml#L36)にGNSS と IMU の周波数を設定する必要があります。

```yaml
common:
  imu_rate: 50
  gnss_rate: 5
```

### 固定からポーズへの変換

`sensor_msgs/msg/NavSatFix`を`geometry_msgs/msg/PoseWithCovarianceStamped`に変換するためのパラメータを[`geo_pose_converter.launch.xml`](https://github.com/MapIV/eagleye/blob/autoware-main/eagleye_util/geo_pose_converter/launch/geo_pose_converter.launch.xml)に示します。
別のジオイドまたは投影タイプを使用する場合は、これらのパラメータを変更します。

### その他のパラメータ

他のパラメータについては[こちら](https://github.com/MapIV/eagleye/tree/autoware-main/eagleye_rt/config)で説明します。
基本的には変更する必要はありません。

## 初期化に関する注意事項

Eagleye を適切に動作させるには、初期化プロセスが必要です。 **初期化しないと、ツイストの出力は生の値になり、ポーズ データは使用できなくなります。**

### 1. 静的初期化

最初のステップは静的初期化です。これには、ヨーレート オフセットを推定するために、起動後約 5 秒間 Eaglelye を静止させておくことが含まれます。

### 2. 動的初期化

次のステップは動的初期化です。これには、Eagleye を約 30 秒間直線で実行することが含まれます。このプロセスでは、車輪速度と方位角のスケール係数を推定します。

動的初期化が完了すると、Eagleye は修正されたツイストとポーズのデータ​​を提供できるようになります。

### 初期化の進行状況を確認する方法

- **TODO**

## 地理参照マップに関する注意事項

適切に地理参照されていないマップを使用している場合、出力位置が点群マップに表示されない可能性があることに注意してください。
単一の GNSS アンテナの場合、GNSS 測位が利用可能な環境で実行を開始してから、初期位置推定 (動的初期化) が完了するまでに数秒かかることがあります。
