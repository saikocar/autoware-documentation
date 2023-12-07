FD-SLAM
FD-SLAMとは何ですか?
FD_SLAM は、Surface Representation Refinement に基づいた、Feature&Distribution ベースの 3D LiDAR SLAM メソッドです。このアルゴリズムでは、高速スキャン マッチングに新しいフィーチャベースの Lidar オドメトリが使用され、キーフレーム マッチングには提案された UGICP メソッドが使用されました。
リポジトリ情報
これは、3D LIDAR を使用したリアルタイム 6DOF SLAM 用のオープンソース ROS パッケージです。

これは hd​​l_graph_slam に基づいており、システムを実行する手順は hdl-graph-slam と同じです。

元のリポジトリのリンク
https://github.com/SLAMWang/FD-SLAM

必要なセンサー
LIDAR[VLP-16、HDL-32、HDL-64、OS1-64]
GPS
IMU [オプション]
ROS の互換性
ROS 1
依存関係
ROS
PCL
g2o
スイートスパース
次の ROS パッケージが必要です。

測地学
nmea_msgs
pcl_ros
ndt_omp
U_gicp fast_gicpをベースに弊社で修正したものです。キーフレームマッチングにはUGICPを使用します。
構築して実行
1) ビルド
cd ~/catkin_ws/src
git clone https://github.com/SLAMWang/FD-SLAM.git
cd ..
catkin_make
2) サービス
/hdl_graph_slam/dump  (hdl_graph_slam/DumpGraph)
  - save all the internal data (point clouds, floor coeffs, odoms, and pose graph) to a directory.

/hdl_graph_slam/save_map (hdl_graph_slam/SaveMap)
  - save the generated map as a PCD file.
3) パラメータを設定する
構成可能なパラメータはすべて、launch/****.launchにros params としてリストされます。
4) 走る
source devel/setup.bash
roslaunch hdl_graph_slam hdl_graph_slam_400_ours.launch

# FD-SLAM

## What is FD-SLAM?

<!-- cspell: ignore UGICP Suitesparse -->

- FD_SLAM is Feature&Distribution-based 3D LiDAR SLAM method based on Surface Representation Refinement. In this algorithm novel feature-based Lidar odometry used for fast scan-matching, and used a proposed UGICP method for keyframe matching.

## Repository Information

This is an open source ROS package for real-time 6DOF SLAM using a 3D LIDAR.

It is based on hdl_graph_slam and the steps to run our system are same with hdl-graph-slam.

### Original Repository link

[https://github.com/SLAMWang/FD-SLAM](https://github.com/SLAMWang/FD-SLAM)

### Required Sensors

- LIDAR[VLP-16, HDL-32, HDL-64, OS1-64]
- GPS
- IMU [Optional]

### ROS Compatibility

- ROS 1

### Dependencies

- [ROS](http://wiki.ros.org/noetic/Installation/Ubuntu)
- [PCL](https://pointclouds.org/downloads/#linux)
- [g2o](http://wiki.ros.org/g2o)
- [Suitesparse](https://github.com/ethz-asl/suitesparse)

The following ROS packages are required:

- geodesy
- nmea_msgs
- pcl_ros
- [ndt_omp](https://github.com/koide3/ndt_omp)
- [U_gicp](https://github.com/SLAMWang/UGICP) This is modified based on [fast_gicp](https://github.com/SMRT-AIST/fast_gicp) by us. We use UGICP for keyframe matching.

## Build & Run

### 1) Build

```bash
cd ~/catkin_ws/src
git clone https://github.com/SLAMWang/FD-SLAM.git
cd ..
catkin_make
```

### 2) Services

```bash
/hdl_graph_slam/dump  (hdl_graph_slam/DumpGraph)
  - save all the internal data (point clouds, floor coeffs, odoms, and pose graph) to a directory.

/hdl_graph_slam/save_map (hdl_graph_slam/SaveMap)
  - save the generated map as a PCD file.
```

### 3) Set parameters

- All the configurable parameters are listed in _launch/\*\*\*\*.launch_ as ros params.

### 4) Run

```bash
source devel/setup.bash
roslaunch hdl_graph_slam hdl_graph_slam_400_ours.launch
```
