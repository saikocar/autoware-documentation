# コンポーネントの依存関係の決定

Autowareをマイクロサービスアーキテクチャとして試したり展開したい開発者は、ソフトウェアの依存関係、通信、および各ROSパッケージ/ノードの実装された機能を理解する必要があります。

例として、Perception コンポーネントの依存関係を確認するために必要なコマンドを以下に示します。

例として知覚コンポーネントの依存関係を確認するために必要なコマンドを以下に示します。

## 知覚コンポーネントの依存関係

パッケージの依存関係のグラフを生成するには次の`colcon`コマンドを使用します:

```bash
colcon graph --dot --packages-up-to tier4_perception_launch | dot -Tpng -o graph.png
```

![colon graph output](images/determining-component-dependencies/perception_stack_dependencies.png){ width="600" }

依存関係のリストを生成するには、以下のコマンドを使用します:

```bash
colcon list --packages-up-to tier4_perception_launch --names-only
```

??? "colcon list output"の例

    ```txt
    autoware_auto_geometry_msgs
    autoware_auto_mapping_msgs
    autoware_auto_perception_msgs
    autoware_auto_planning_msgs
    autoware_auto_vehicle_msgs
    autoware_cmake
    autoware_lint_common
    autoware_point_types
    compare_map_segmentation
    detected_object_feature_remover
    detected_object_validation
    detection_by_tracker
    euclidean_cluster
    grid_map_cmake_helpers
    grid_map_core
    grid_map_cv
    grid_map_msgs
    grid_map_pcl
    grid_map_ros
    ground_segmentation
    image_projection_based_fusion
    image_transport_decompressor
    interpolation
    kalman_filter
    lanelet2_extension
    lidar_apollo_instance_segmentation
    map_based_prediction
    multi_object_tracker
    mussp
    object_merger
    object_range_splitter
    occupancy_grid_map_outlier_filter
    pointcloud_preprocessor
    pointcloud_to_laserscan
    shape_estimation
    tensorrt_yolo
    tier4_autoware_utils
    tier4_debug_msgs
    tier4_pcl_extensions
    tier4_perception_launch
    tier4_perception_msgs
    traffic_light_classifier
    traffic_light_map_based_detector
    traffic_light_ssd_fine_detector
    traffic_light_visualization
    vehicle_info_util
    ```

!!! ヒント

    モジュールのリストとそれぞれのパスを出力するには、`--names-only` パラメーターを指定せずに上記のコマンドを実行します。

どのROSトピックがサブスクライブおよび公開されているかを確認するには、`rqt_graph`を次のように使用します:

```bash
ros2 launch tier4_perception_launch perception.launch.xml mode:=lidar
ros2 run rqt_graph rqt_graph
```

<!-- TODO: Add a way of determining software dependencies -->
