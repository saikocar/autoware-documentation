地図
ノード図

概要
Autoware は、運転環境の高解像度点群マップとベクトル マップを利用してさまざまなタスクを実行します。Autoware を起動する前に、事前に作成されたマップ ファイルをロードする必要があります。

入力
点群マップ ( .pcd)
Lanelet2 マップ ( .osm)
マップの作成方法については、「マップの作成」を参照してください。

出力
点群マップ
点群ファイルをロードし、さまざまな構成で他の Autoware ノードにマップを公開します。現在、次のタイプがサポートされています。

生の点群マップ (sensor_msgs/msg/PointCloud2)
ダウンサンプリングされた点群マップ (sensor_msgs/msg/PointCloud2)
ROS サービスを介した部分点群マップの読み込み (autoware_map_msgs/srv/GetPartialPointCloudMap)
ROS サービス経由の差分点群マップの読み込み (autoware_map_msgs/srv/GetDifferentialPointCloudMap)
レーンレット2マップ
Lanelet2 ファイルを読み込み、地図データをautoware_auto_mapping_msgs/msg/HADMapBinメッセージとして公開します。lan/lon 座標は MGRS 座標に投影されます。

autoware_auto_mapping_msgs/msg/HADMapBin
std_msgs/Header ヘッダー
文字列 version_map_format
文字列バージョンマップ
文字列名_マップ
uint8[] データ
Lanelet2 マップの視覚化
autoware_auto_mapping_msgs/HADMapBinのメッセージを視覚化しますRviz。

視覚化_msgs/msg/MarkerArray
# Map

![Node diagram](./images/Map-Bus-ODD-Architecture.drawio.svg)

## Overview

Autoware relies on high-definition point cloud maps and vector maps of the driving environment to perform various tasks. Before launching Autoware, you need to load the pre-created map files.

## Inputs

- Point cloud maps (`.pcd`)
- Lanelet2 maps (`.osm`)

Refer to [Creating maps](../../../how-to-guides/integrating-autoware/creating-maps/index.md) on how to create maps.

## Outputs

### Point cloud map

It loads point cloud files and publishes the maps to the other Autoware nodes in various configurations. Currently, it supports the following types:

- Raw point cloud map (sensor_msgs/msg/PointCloud2)
- Downsampled point cloud map (sensor_msgs/msg/PointCloud2)
- Partial point cloud map loading via ROS service (autoware_map_msgs/srv/GetPartialPointCloudMap)
- Differential point cloud map loading via ROS service (autoware_map_msgs/srv/GetDifferentialPointCloudMap)

### Lanelet2 map

It loads a Lanelet2 file and publishes the map data as `autoware_auto_mapping_msgs/msg/HADMapBin` message. The lan/lon coordinates are projected onto the MGRS coordinates.

- autoware_auto_mapping_msgs/msg/HADMapBin
  - std_msgs/Header header
  - string version_map_format
  - string version_map
  - string name_map
  - uint8[] data

### Lanelet2 map visualization

Visualize `autoware_auto_mapping_msgs/HADMapBin` messages in `Rviz`.

- visualization_msgs/msg/MarkerArray
