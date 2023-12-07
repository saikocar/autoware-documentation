センシングコンポーネントの設計
概要
センシング コンポーネントは、生のセンサー データにいくつかの原始的な前処理を適用するモジュールのコレクションです。

センサーの入力形式はこのコンポーネントで定義されます。

役割
データ形式の抽象化により、さまざまなベンダーのセンサーを使用できるようになります。
各コンポーネントに必要な共通/原始的なセンサー データ処理を実行します。
入力
入力の種類
センサーデータ	メッセージの種類
点群 (LIDAR、深度カメラなど)	センサー_msgs/msg/PointCloud2.msg
画像（RGB、モノクロ、深度などのカメラ）	センサー_msgs/msg/Image.msg
レーダースキャン	レーダー_msgs/msg/RadarScan.msg
レーダー追跡	レーダー_msgs/msg/RadarTracks.msg
GNSS-INS位置	sensor_msgs/msg/NavSatFix.msg
GNSS-INS の方向	autoware_sensing_msgs/GnssInsOrientationStamped.msg
GNSS-INS速度	geometry_msgs/msg/TwistWithCovarianceStamped.msg
GNSS-INS加速	geometry_msgs/msg/AccelWithCovarianceStamped.msg
超音波	センサー_msgs/msg/Range.msg
データ型による設計
GNSS/INSデータ前処理設計
画像前処理設計
点群前処理設計
レーダーデータの前処理設計
超音波データ前処理設計
# Sensing component design

## Overview

Sensing component is a collection of modules that apply some primitive pre-processing to the raw sensor data.

The sensor input formats are defined in this component.

## Role

- Abstraction of data formats to enable usage of sensors from various vendors
- Perform common/primitive sensor data processing required by each component

## Inputs

### Input types

| Sensor Data                                  | Message Type                                                                                                                                                                 |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Point cloud (Lidars, depth cameras, etc.)    | [sensor_msgs/msg/PointCloud2.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/PointCloud2.msg)                                                    |
| Image (RGB, monochrome, depth, etc. cameras) | [sensor_msgs/msg/Image.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Image.msg)                                                                |
| Radar scan                                   | [radar_msgs/msg/RadarScan.msg](https://github.com/ros-perception/radar_msgs/blob/ros2/msg/RadarScan.msg)                                                                     |
| Radar tracks                                 | [radar_msgs/msg/RadarTracks.msg](https://github.com/ros-perception/radar_msgs/blob/ros2/msg/RadarTracks.msg)                                                                 |
| GNSS-INS position                            | [sensor_msgs/msg/NavSatFix.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/NavSatFix.msg)                                                        |
| GNSS-INS orientation                         | [autoware_sensing_msgs/GnssInsOrientationStamped.msg](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_sensing_msgs/msg/GnssInsOrientationStamped.msg) |
| GNSS-INS velocity                            | [geometry_msgs/msg/TwistWithCovarianceStamped.msg](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/TwistWithCovarianceStamped.msg)                  |
| GNSS-INS acceleration                        | [geometry_msgs/msg/AccelWithCovarianceStamped.msg](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/AccelWithCovarianceStamped.msg)                  |
| Ultrasonics                                  | [sensor_msgs/msg/Range.msg](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Range.msg)                                                                |

## Design by data-types

- [GNSS/INS data pre-processing design](data-types/gnss-ins-data.md)
- [Image pre-processing design](data-types/image.md)
- [Point cloud pre-processing design](data-types/point-cloud.md)
- [Radar data pre-processing design](data-types/radar-data.md)
- [Ultrasonics data pre-processing design](data-types/ultrasonics-data.md)
