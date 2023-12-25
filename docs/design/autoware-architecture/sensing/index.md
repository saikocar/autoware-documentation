# 計測コンポーネントの設計

## 概要

計測コンポーネントは、生のセンサーデータにいくつかの原始的な前処理を適用するモジュールの集まりです。

センサーの入力形式はこのコンポーネントで定義されます。

## 役割

- データ形式の抽象化により、さまざまなベンダーのセンサーを使用できるようになります。
- 各コンポーネントに必要な共通/原始的なセンサー データ処理を実行します。
## 入力

### 入力の型

| センサーデータ                                  | メッセージの型                                                                                                                                                                 |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 点群(Lidar、深度カメラ等)    | [sensor_msgs/msg/PointCloud2.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/PointCloud2.msg)                                                    |
| 画像(RGB、モノクロ、深度等のカメラ) | [sensor_msgs/msg/Image.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Image.msg)                                                                |
| レーダースキャン                                   | [radar_msgs/msg/RadarScan.msg](https://github.com/ros-perception/radar_msgs/blob/ros2/msg/RadarScan.msg)                                                                     |
| レーダー追跡                                 | [radar_msgs/msg/RadarTracks.msg](https://github.com/ros-perception/radar_msgs/blob/ros2/msg/RadarTracks.msg)                                                                 |
| GNSS-INS位置                            | [sensor_msgs/msg/NavSatFix.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/NavSatFix.msg)                                                        |
| GNSS-INS方向                         | [autoware_sensing_msgs/GnssInsOrientationStamped.msg](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_sensing_msgs/msg/GnssInsOrientationStamped.msg) |
| GNSS-INS速度                            | [geometry_msgs/msg/TwistWithCovarianceStamped.msg](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/TwistWithCovarianceStamped.msg)                  |
| GNSS-INS加速度                        | [geometry_msgs/msg/AccelWithCovarianceStamped.msg](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/AccelWithCovarianceStamped.msg)                  |
| 超音波                                  | [sensor_msgs/msg/Range.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Range.msg)                                                                |

## データ型による設計

- [GNSS/INSデータ前処理設計](data-types/gnss-ins-data.md)
- [画像前処理設計](data-types/image.md)
- [点群前処理設計](data-types/point-cloud.md)
- [レーダーデータの前処理設計](data-types/radar-data.md)
- [超音波データ前処理設計](data-types/ultrasonics-data.md)
