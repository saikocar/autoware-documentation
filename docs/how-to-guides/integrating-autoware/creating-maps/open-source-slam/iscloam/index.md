スクローム
イスクロアムとは何ですか？
ISCLOAM は、ジオメトリ情報と強度情報の両方を統合することにより、堅牢なループ閉鎖検出アプローチを提供します。
リポジトリ情報
元のリポジトリのリンク
https://github.com/wh200720041/iscloam

必要なセンサー
LIDAR [ベロダイン]
ROS の互換性
ROS 1
依存関係
Ubuntu 64 ビット 18.04
ROS Melodic ROS のインストール
Ceres ソルバーCeres のインストール
PCL PCL のインストール
Gtsam GTSAM のインストール
OpenCV OPENCV のインストール
軌跡の可視化
視覚化の目的で、このパッケージは hector trajectory サーバーを使用します。次の方法でパッケージをインストールできます。

sudo apt-get install ros-melodic-hector-trajectory-server
構築して実行する
1. リポジトリのクローンを作成する
cd ~/catkin_ws/src
git clone https://github.com/wh200720041/iscloam.git
cd ..
catkin_make -j1
source ~/catkin_ws/devel/setup.bash
2. パラメータの設定
起動ファイルのバッグの位置とセンサーのパラメーターを変更します。



3. 起動
roslaunch iscloam iscloam.launch
同時に環境のマップを生成したい場合は、次を実行できます。

roslaunch iscloam iscloam_mapping.launch
グローバル マップは非常に大きくなる可能性があるため、グローバル最適化の実行に時間がかかる場合があることに注意してください。軌道とマップは別のスレッドで実行されるため、両者の間に多少の遅れが予想されます。ループの終了が特定されると、より多くの CPU 使用率が発生します。

結果の例
ビデオリンクでデモビデオをご覧ください

グラウンドトゥルースの比較
緑：イスクローム 赤：グラウンドトゥルース

 

                  KITTI sequence 00                                  KITTI sequence 05
引用
この作品を研究に使用する場合は、以下の論文を引用してください。引用していただければ幸いです。

@inproceedings{wang2020intensity,
  author={H. {Wang} and C. {Wang} and L. {Xie}},
  booktitle={2020 IEEE International Conference on Robotics and Automation (ICRA)},
  title={Intensity Scan Context: Coding Intensity and Geometry Relations for Loop Closure Detection},
  year={2020},
  volume={},
  number={},
  pages={2095-2101},
  doi={10.1109/ICRA40945.2020.9196764}
}
謝辞
A-LOAMと LOAM(J. Zhang と S. Singh. LOAM: Lidar Odometry and Mapping in Real-time) とLOAM_NOTEDに感謝します。

著者: Wang Han、南洋理工大学、シンガポール
# ISCLOAM

## What is ISCLOAM?

- ISCLOAM presents a robust loop closure detection approach by integrating both geometry and intensity information.

## Repository Information

### Original Repository link

[https://github.com/wh200720041/iscloam](https://github.com/wh200720041/iscloam)

### Required Sensors

- LIDAR [Velodyne]

### ROS Compatibility

- ROS 1

### Dependencies

- Ubuntu 64-bit 18.04
- ROS Melodic [ROS Installation](http://wiki.ros.org/ROS/Installation)
- Ceres Solver [Ceres Installation](http://ceres-solver.org/installation.html)
- PCL [PCL Installation](https://pointclouds.org/downloads/)
- Gtsam [GTSAM Installation](https://gtsam.org/get_started/)
- OpenCV [OPENCV Installation](https://opencv.org/releases/)
- Trajectory visualization

For visualization purpose, this package uses hector trajectory sever, you may install the package by

```bash
sudo apt-get install ros-melodic-hector-trajectory-server
```

## Build and Run

### 1. Clone repository

```bash
cd ~/catkin_ws/src
git clone https://github.com/wh200720041/iscloam.git
cd ..
catkin_make -j1
source ~/catkin_ws/devel/setup.bash
```

### 2. Set Parameter

Change the bag location and sensor parameters on launch files.

<p><img src="images/bag_name.png" width=800></p>

### 3. Launch

```bash
roslaunch iscloam iscloam.launch
```

if you would like to generate the map of environment at the same time, you can run

```bash
roslaunch iscloam iscloam_mapping.launch
```

Note that the global map can be very large, so it may takes a while to perform global optimization, some lag is expected between trajectory and map since they are running in separate thread. More CPU usage will happen when loop closure is identified.

## Example Result

Watch demo video at [Video Link](https://youtu.be/Kfi6CFK4Ke4)

### Ground Truth Comparison

Green: ISCLOAM Red: Ground Truth

<p>
<img src="images/00.png" width = 45% />
<img src="images/05.png" width = 45% />
</p>

                      KITTI sequence 00                                  KITTI sequence 05

## Citation

If you use this work for your research, you may want to cite the paper below, your citation will be appreciated

```bash
@inproceedings{wang2020intensity,
  author={H. {Wang} and C. {Wang} and L. {Xie}},
  booktitle={2020 IEEE International Conference on Robotics and Automation (ICRA)},
  title={Intensity Scan Context: Coding Intensity and Geometry Relations for Loop Closure Detection},
  year={2020},
  volume={},
  number={},
  pages={2095-2101},
  doi={10.1109/ICRA40945.2020.9196764}
}
```

## Acknowledgements

Thanks for [A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM) and LOAM(J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time) and [LOAM_NOTED](https://github.com/cuitaixiang/LOAM_NOTED).

**Author:** [Wang Han](http://wanghan.pro), Nanyang Technological University, Singapore
