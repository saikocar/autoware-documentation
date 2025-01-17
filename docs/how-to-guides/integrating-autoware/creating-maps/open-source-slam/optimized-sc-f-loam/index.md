最適化されたSC-F-LOAM
最適化された SC-F-LOAM とは何ですか?
F-LOAM の改良版であり、適応しきい値を使用してループ クロージャの検出結果をさらに判断し、誤ったループ クロージャの検出を減らします。また、特徴点ベースのマッチングを使用して、ループ クロージャー フレーム点群のペア間の制約を計算し、ループ フレーム制約の構築にかかる時間を削減します。
リポジトリ情報
元のリポジトリのリンク
https://github.com/SlamCabbage/Optimized-SC-F-LOAM

必要なセンサー
LIDAR [VLP-16、HDL-32、HDL-64]
ROS の互換性
ROS 1
依存関係
ROS
PCL
GTSAM
セレスソルバー
視覚化の目的で、このパッケージは hector trajectory サーバーを使用します。次の方法でパッケージをインストールできます。
sudo apt-get install ros-noetic-hector-trajectory-server
構築して実行
1) ビルド
cd ~/catkin_ws/src
git clone https://github.com/SlamCabbage/Optimized-SC-F-LOAM.git
cd ..
catkin_make
2) メッセージファイルの作成
このフォルダには、Ground Truth情報、最適化されたポーズ情報、F-LOAMのポーズ情報、時間情報が格納されます。

mkdir -p ~/message/Scans

Change line 383 in the laserLoopOptimizationNode.cpp to your own "message" folder path
(パッケージを再構築することを忘れないでください)

3) パラメータを設定する
「sc_f_loam_mapping.launch」で LIDAR トピックと LIDAR プロパティを設定します
4) 走る
source devel/setup.bash
roslaunch optimized_sc_f_loam optimized_sc_f_loam_mapping.launch
結果の例
画像

KITTI シーケンス 00 およびシーケンス 05 の結果
画像

KITTI データセット上の軌跡の比較
画像

KITTI シーケンスでのテスト シーケンス 00 および 05 データセットを KITTI 公式 Web サイトからダウンロードし、kitti2bag オープン ソース メソッドを使用してバッグ ファイルに変換できます。

00: 2011_10_03_ドライブ_0027 000000 004540

05: 2011_09_30_drive_0018 000000 002760

リンクを参照してください: https://github.com/ethz-asl/kitti_to_rosbag

謝辞
SC-A-LOAM (スキャンコンテキスト: 3D 点群マップ内の場所認識のための自己中心的空間記述子) と F-LOAM (F-LOAM : Fast LiDAR Odometry and Mapping) に感謝します。

引用
@misc{https://doi.org/10.48550/arxiv.2204.04932,
  doi = {10.48550/ARXIV.2204.04932},

  url = {https://arxiv.org/abs/2204.04932},

  author = {Liao, Lizhou and Fu, Chunyun and Feng, Binbin and Su, Tian},

  keywords = {Robotics (cs.RO), FOS: Computer and information sciences, FOS: Computer and information sciences},

  title = {Optimized SC-F-LOAM: Optimized Fast LiDAR Odometry and Mapping Using Scan Context},

  publisher = {arXiv},

  year = {2022},

  copyright = {arXiv.org perpetual, non-exclusive license}
}
# Optimized-SC-F-LOAM

## What is Optimized-SC-F-LOAM?

- An improved version of F-LOAM and uses an adaptive threshold to further judge the loop closure detection results and reducing false loop closure detections. Also it uses feature point-based matching to calculate the constraints between a pair of loop closure frame point clouds and decreases time consumption of constructing loop frame constraints.

## Repository Information

### Original Repository link

[https://github.com/SlamCabbage/Optimized-SC-F-LOAM](https://github.com/SlamCabbage/Optimized-SC-F-LOAM)

### Required Sensors

- LIDAR [VLP-16, HDL-32, HDL-64]

### ROS Compatibility

- ROS 1

### Dependencies

- [ROS](http://wiki.ros.org/noetic/Installation/Ubuntu)
- [PCL](https://pointclouds.org/downloads/#linux)
- [GTSAM](https://gtsam.org/get_started/)
- [Ceres Solver](http://www.ceres-solver.org/installation.html)
- For visualization purpose, this package uses hector trajectory sever, you may install the package by

```bash
sudo apt-get install ros-noetic-hector-trajectory-server
```

## Build & Run

### 1) Build

```bash
cd ~/catkin_ws/src
git clone https://github.com/SlamCabbage/Optimized-SC-F-LOAM.git
cd ..
catkin_make
```

### 2) Create message file

In this folder, Ground Truth information, optimized pose information, F-LOAM pose information and time information are stored

```bash
mkdir -p ~/message/Scans

Change line 383 in the laserLoopOptimizationNode.cpp to your own "message" folder path
```

(Do not forget to rebuild your package)

### 3) Set parameters

- Set LIDAR topic and LIDAR properties on 'sc_f_loam_mapping.launch'

### 4) Run

```bash
source devel/setup.bash
roslaunch optimized_sc_f_loam optimized_sc_f_loam_mapping.launch
```

## Example Result

![image](https://user-images.githubusercontent.com/95751923/155124889-934ea649-3b03-4e8d-84af-608753f34c93.png)

### Results on KITTI Sequence 00 and Sequence 05

![image](https://user-images.githubusercontent.com/95751923/155125294-980e6a3d-6e76-4a23-9771-493ba278677e.png)

### Comparison of trajectories on KITTI dataset

![image](https://user-images.githubusercontent.com/95751923/155125478-a361762f-f18e-4161-b892-6f5080f5681f.png)

Test on KITTI sequence
You can download the sequence 00 and 05 datasets from the KITTI official website and convert them into bag files using the kitti2bag open source method.

00: 2011_10_03_drive_0027 000000 004540

05: 2011_09_30_drive_0018 000000 002760

See the link: [https://github.com/ethz-asl/kitti_to_rosbag](https://github.com/ethz-asl/kitti_to_rosbag)

## Acknowledgements

Thanks for SC-A-LOAM(Scan context: Egocentric spatial descriptor for place recognition within 3d point cloud map) and F-LOAM(F-LOAM : Fast LiDAR Odometry and Mapping).

## Citation

```bash
@misc{https://doi.org/10.48550/arxiv.2204.04932,
  doi = {10.48550/ARXIV.2204.04932},

  url = {https://arxiv.org/abs/2204.04932},

  author = {Liao, Lizhou and Fu, Chunyun and Feng, Binbin and Su, Tian},

  keywords = {Robotics (cs.RO), FOS: Computer and information sciences, FOS: Computer and information sciences},

  title = {Optimized SC-F-LOAM: Optimized Fast LiDAR Odometry and Mapping Using Scan Context},

  publisher = {arXiv},

  year = {2022},

  copyright = {arXiv.org perpetual, non-exclusive license}
}
```
