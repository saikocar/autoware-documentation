CUDA を使用せずに Autoware を実行する
Autoware Universe での物体検出と信号機認識のパフォーマンスを向上させるには CUDA のインストールが推奨されますが、CUDA なしでこれらのアルゴリズムを実行することも可能です。次のサブセクションでは、そのような環境で各アルゴリズムを実行する方法を簡単に説明します。

CUDA を使用しない 2D/3D オブジェクト検出の実行
Autoware Universe のオブジェクト検出は、次の 5 つの構成のいずれかを使用して実行できます。

lidar_centerpoint
lidar_apollo_instance_segmentation
lidar-apollo+tensorrt_yolo
lidar-centerpoint+tensorrt_yolo
euclidean_cluster
euclidean_clusterこれら 5 つの構成のうち、 CUDA なしで実行できるのは最後の構成 ( ) だけです。詳細については、euclidean_clusterモジュールの README ファイルを参照してください。

CUDA を使用しないでの信号検出の実行
交通信号認識 (検出と分類の両方) には、CUDA を必要とする 2 つのモジュールがあります。

traffic_light_ssd_fine_detector
traffic_light_classifier
CUDA を使用せずに信号機検出を実行するには、信号機起動ファイルでenable_fine_detectionをfalse設定します。これを行うと、 が無効になりtraffic_light_ssd_fine_detector、代わりにモジュールによって信号検出が処理されるようになりますmap_based_traffic_light_detector。

CUDA を使用せずに信号機分類を実行するには、信号機分類子起動ファイルでuse_gpuをfalse設定します。traffic_light_classifierこれを行うと、 は CUDA や GPU を必要としない別の分類アルゴリズムを使用することになります。
# Running Autoware without CUDA

Although CUDA installation is recommended to achieve better performance for object detection and traffic light recognition in Autoware Universe, it is possible to run these algorithms without CUDA.
The following subsections briefly explain how to run each algorithm in such an environment.

## Running 2D/3D object detection without CUDA

Autoware Universe's object detection can be run using one of five possible configurations:

- `lidar_centerpoint`
- `lidar_apollo_instance_segmentation`
- `lidar-apollo` + `tensorrt_yolo`
- `lidar-centerpoint` + `tensorrt_yolo`
- `euclidean_cluster`

Of these five configurations, only the last one (`euclidean_cluster`) can be run without CUDA. For more details, refer to the [`euclidean_cluster` module's README file](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/euclidean_cluster).

## Running traffic light detection without CUDA

For traffic light recognition (both detection and classification), there are two modules that require CUDA:

- `traffic_light_ssd_fine_detector`
- `traffic_light_classifier`

To run traffic light detection without CUDA, set [`enable_fine_detection` to `false` in the traffic light launch file](https://github.com/autowarefoundation/autoware.universe/blob/9445f3a7acd645d12a64507c3d3bfa57e74a3634/launch/tier4_perception_launch/launch/traffic_light_recognition/traffic_light.launch.xml#L3). Doing so disables the `traffic_light_ssd_fine_detector` such that traffic light detection is handled by the `map_based_traffic_light_detector` module instead.

To run traffic light classification without CUDA, set [`use_gpu` to `false` in the traffic light classifier launch file](https://github.com/autowarefoundation/autoware.universe/blob/9445f3a7acd645d12a64507c3d3bfa57e74a3634/perception/traffic_light_classifier/launch/traffic_light_classifier.launch.xml#L7). Doing so will force the `traffic_light_classifier` to use a different classification algorithm that does not require CUDA or a GPU.
