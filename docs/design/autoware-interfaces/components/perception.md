感知
このページでは、Perception コンポーネントのインターフェイスに関する具体的な仕様を提供します。概念とデータ フローについては、認識アーキテクチャのリファレンス実装設計ドキュメントを参照してください。

入力
マップコンポーネントから
名前	トピックス・サービス	タイプ	説明
ベクトルマップ	/map/vector_map	autoware_auto_mapping_msgs/msg/HADMapBin	車線情報を含むHDマップ
点群マップ	/service/get_differential_pcd_map	autoware_map_msgs/srv/GetDifferentialPointCloudMap	点群マップ
ノート：

点群マップ
input はトピックまたはサービスの両方にすることができますが、このインターフェイスによりマップ ファイル サイズの制限に制約されずに処理できるため、サービスの使用を強くお勧めします。
センシングコンポーネントから
名前	トピック	タイプ	説明
カメラ画像	/sensing/camera/camera*/image_rect_color	センサーメッセージ/画像	レンズ歪み補正 (LDC) で処理されたカメラ画像データ
カメラ画像	/sensing/camera/camera*/image_raw	センサーメッセージ/画像	レンズ歪み補正 (LDC) 処理されていないカメラ画像データ
点群	/sensing/lidar/concatenated/pointcloud	センサー_msgs/PointCloud2	複数の LiDAR ソースからの点群を連結
レーダーオブジェクト	/sensing/radar/detected_objects	autoware_auto_perception_msgs/msg/DetectedObject	レーダーオブジェクト
ローカリゼーションコンポーネントから
名前	トピック	タイプ	説明
車両オドメトリ	/localization/kinematic_state	nav_msgs/msg/オドメトリ	自家車両オドメトリ トピック
APIから
名前	トピック	タイプ	説明
外部交通信号機	/external/traffic_signals	autoware_perception_msgs::msg::TrafficSignalArray	外部システムからの信号機
出力
企画へ
名前	トピック	タイプ	説明
動的オブジェクト	/perception/object_recognition/objects	autoware_auto_perception_msgs/msg/PredictedObjects	オブジェクト クラスやオブジェクトの形状などの情報を含む動的オブジェクトのセット
障害物	/perception/obstacle_segmentation/pointcloud	センサー_msgs/PointCloud2	障害物 (動的オブジェクトと静的オブジェクトを含む)
占有グリッドマップ	/perception/occupancy_grid_map/map	nav_msgs/msg/OccupancyGrid	障害物の存在や死角に関する情報を含む地図
信号機	/perception/traffic_light_recognition/traffic_signals	autoware_perception_msgs::msg::TrafficSignalArray	色（緑、黄、読み）や矢印（右、左、直進）などの信号情報
# Perception

This page provides specific specifications about the Interface of the Perception Component. Please refer to [the perception architecture reference implementation design document](../../autoware-architecture/perception/reference_implementation.md) for concepts and data flow.

## Input

### From Map Component

| Name            | Topic / Service                     | Type                                                                                                                                                                       | Description                                  |
| --------------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| Vector Map      | `/map/vector_map`                   | [autoware_auto_mapping_msgs/msg/HADMapBin](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_mapping_msgs/msg/HADMapBin.idl)                       | HD Map including the information about lanes |
| Point Cloud Map | `/service/get_differential_pcd_map` | [autoware_map_msgs/srv/GetDifferentialPointCloudMap](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_map_msgs/srv/GetDifferentialPointCloudMap.srv) | Point Cloud Map                              |

Notes:

- Point Cloud Map
  - input can be both topic or service, but we highly recommend to use service because since this interface enables processing without being constrained by map file size limits.

### From Sensing Component

| Name         | Topic                                      | Type                                                                                                                                                                                          | Description                                                            |
| ------------ | ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Camera Image | `/sensing/camera/camera*/image_rect_color` | [sensor_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg)                                                                                          | Camera image data, processed with Lens Distortion Correction (LDC)     |
| Camera Image | `/sensing/camera/camera*/image_raw`        | [sensor_msgs/Image](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg)                                                                                          | Camera image data, not processed with Lens Distortion Correction (LDC) |
| Point Cloud  | `/sensing/lidar/concatenated/pointcloud`   | [sensor_msgs/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg)                                                                              | Concatenated point cloud from multiple LiDAR sources                   |
| Radar Object | `/sensing/radar/detected_objects`          | [autoware_auto_perception_msgs/msg/DetectedObject](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/DetectedObject.idl) | Radar objects                                                          |

### From Localization Component

| Name             | Topic                           | Type                                                                                                     | Description                |
| ---------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------- |
| Vehicle Odometry | `/localization/kinematic_state` | [nav_msgs/msg/Odometry](https://github.com/ros2/common_interfaces/blob/humble/nav_msgs/msg/Odometry.msg) | Ego vehicle odometry topic |

### From API

| Name                     | Topic                       | Type                                                                                                                                                                   | Description                                 |
| ------------------------ | --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| External Traffic Signals | `/external/traffic_signals` | [autoware_perception_msgs::msg::TrafficSignalArray](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_perception_msgs/msg/TrafficSignalArray.msg) | The traffic signals from an external system |

## Output

### To Planning

| Name               | Topic                                                   | Type                                                                                                                                                                     | Description                                                                                               |
| ------------------ | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| Dynamic Objects    | `/perception/object_recognition/objects`                | [autoware_auto_perception_msgs/msg/PredictedObjects](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_perception_msgs/msg/PredictedObjects.idl) | Set of dynamic objects with information such as a object class and a shape of the objects                 |
| Obstacles          | `/perception/obstacle_segmentation/pointcloud`          | [sensor_msgs/PointCloud2](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg)                                                         | Obstacles, which includes dynamic objects and static objetcs                                              |
| Occupancy Grid Map | `/perception/occupancy_grid_map/map`                    | [nav_msgs/msg/OccupancyGrid](https://docs.ros.org/en/latest/api/nav_msgs/html/msg/OccupancyGrid.html)                                                                    | The map with the imformation about the presence of obstacles and blind spot                               |
| Traffic Signal     | `/perception/traffic_light_recognition/traffic_signals` | [autoware_perception_msgs::msg::TrafficSignalArray](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_perception_msgs/msg/TrafficSignalArray.msg)   | The traffic signal information such as a color (green, yellow, read) and an arrow (right, left, straight) |
