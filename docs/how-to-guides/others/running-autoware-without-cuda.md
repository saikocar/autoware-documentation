# CUDAを使用せずにAutowareを実行する

Autoware Universeでの物体検出と信号機認識のパフォーマンスを向上させるにはCUDAのインストールが推奨されますが、CUDAなしでこれらのアルゴリズムを実行することも可能です。
次のサブセクションでは、そのような環境で各アルゴリズムを実行する方法を簡単に説明します。

## CUDAを使用しない2D/3Dオブジェクト検出の実行

Autoware Universeのオブジェクト検出は、次の5つの構成のいずれかを使用して実行できます:

- `lidar_centerpoint`
- `lidar_apollo_instance_segmentation`
- `lidar-apollo` + `tensorrt_yolo`
- `lidar-centerpoint` + `tensorrt_yolo`
- `euclidean_cluster`

これら5つの構成のうち、CUDAなしで実行できるのは最後の構成(`euclidean_cluster`)だけです。詳細については[`euclidean_cluster`モジュールのREADMEファイル](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/euclidean_cluster)を参照してください。

## CUDAを使用しないで信号検出を実行する

交通信号認識(検出と分類の両方)には、CUDAを必要とする2つのモジュールがあります:

- `traffic_light_ssd_fine_detector`
- `traffic_light_classifier`

CUDAを使用せずに信号機検出を実行するには、 set [信号機起動ファイルで`enable_fine_detection`を`false`に](https://github.com/autowarefoundation/autoware.universe/blob/9445f3a7acd645d12a64507c3d3bfa57e74a3634/launch/tier4_perception_launch/launch/traffic_light_recognition/traffic_light.launch.xml#L3)設定します。これを行うと`traffic_light_ssd_fine_detector` が無効になり、代わりに`map_based_traffic_light_detector`モジュールによって信号検出が処理されるようになります。

CUDAを使用せずに信号機分類を実行するには、信号機分類子起動ファイルでuse_gpuをfalse設定します。traffic_light_classifierこれを行うと、 は CUDA や GPU を必要としない別の分類アルゴリズムを使用することになります。To run traffic light classification without CUDA, set [信号機分類子起動ファイルで`use_gpu`を`false`に](https://github.com/autowarefoundation/autoware.universe/blob/9445f3a7acd645d12a64507c3d3bfa57e74a3634/perception/traffic_light_classifier/launch/traffic_light_classifier.launch.xml#L7)設定します。これを行うと、`traffic_light_classifier`はCUDAやGPUを必要としない別の分類アルゴリズムを使用することになります。
