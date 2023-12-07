FAST_LIO_LC
FAST_LIO_LCとは何ですか?
ループ クロージャ モジュールとグラフ最適化を備えた、計算効率が高く堅牢な LiDAR 慣性オドメトリ パッケージ。
リポジトリ情報
元のリポジトリのリンク
https://github.com/yanliang-wang/FAST_LIO_LC

必要なセンサー
LIDAR [ベロダイン、オースター、リボックス]
IMU [6軸、9軸]
GPS [オプション]
ROS の互換性
ROS 1
依存関係
Ubuntu 18.04
ROSメロディック
PCL >= 1.8、PCL のインストールに従ってください。
Eigen >= 3.3.4、Eigen のインストールに従ってください。
GTSAM >= 4.0.0、GTSAM のインストールに従ってください。
  wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
  cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
  cd ~/Downloads/gtsam-4.0.0-alpha2/
  mkdir build && cd build
  cmake ..
  sudo make install
構築して実行
1) ビルド
    mkdir -p ~/ws_fastlio_lc/src
    cd ~/ws_fastlio_lc/src
    git clone https://github.com/gisbi-kim/FAST_LIO_SLAM.git
    git clone https://github.com/Livox-SDK/livox_ros_driver
    cd ..
    catkin_make
2) パラメータの設定
リポジトリをダウンロードした後、workspace/src/FAST_LIO_LC/FAST_LIO/config/ouster64_mulran.yamlbag ファイル内の LIDAR トピック名を使用して、構成ファイル ( ) のトピックとセンサーの設定を変更します。


imu-lidar との互換性を確保するには、キャリブレーションの外部行列を変更する必要があります。
外部行列

自動保存を有効にするには、起動ファイル ( ) からpcd_save_enable保存する必要があります。1workspace/src/FAST_LIO_LC/FAST_LIO/launch/mapping_ouster64_mulran.launch
3) 走る
オースターOS1-64用

# open new terminal: run FAST-LIO
roslaunch fast_lio mapping_ouster64.launch

# open the other terminal tab: run SC-PGO
roslaunch aloam_velodyne fastlio_ouster64.launch

# play bag file in the other terminal
rosbag play RECORDED_BAG.bag --clock
打ち上げ

結果の例
例_結果1

例_結果2

その他の例
例_結果

データセットの例
サンプル データセットの元のリポジトリ リンクを確認してください。

接触
管理者: 王燕良 ( wyl410922@qq.com)
謝辞
FAST_LIO の作者に感謝します。
# FAST_LIO_LC

## What is FAST_LIO_LC?

- A computationally efficient and robust LiDAR-inertial odometry package with loop closure module and graph optimization.

## Repository Information

### Original Repository link

[https://github.com/yanliang-wang/FAST_LIO_LC](https://github.com/yanliang-wang/FAST_LIO_LC)

### Required Sensors

- LIDAR [Velodyne, Ouster, Livox]
- IMU [6-AXIS, 9-AXIS]
- GPS [Optional]

### ROS Compatibility

- ROS 1

### Dependencies

- Ubuntu 18.04
- ROS Melodic
- PCL >= 1.8, Follow [PCL Installation](https://pointclouds.org/downloads/#linux).
- Eigen >= 3.3.4, Follow [Eigen Installation](http://eigen.tuxfamily.org/index.php?title=Main_Page).
- GTSAM >= 4.0.0, Follow [GTSAM Installation](https://gtsam.org/get_started).

```bash
  wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
  cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
  cd ~/Downloads/gtsam-4.0.0-alpha2/
  mkdir build && cd build
  cmake ..
  sudo make install
```

## Build & Run

### 1) Build

```bash
    mkdir -p ~/ws_fastlio_lc/src
    cd ~/ws_fastlio_lc/src
    git clone https://github.com/gisbi-kim/FAST_LIO_SLAM.git
    git clone https://github.com/Livox-SDK/livox_ros_driver
    cd ..
    catkin_make
```

### 2) Set parameters

<!-- cspell: ignore mulran -->

- After downloading the repository, change topic and sensor settings on the config file (`workspace/src/FAST_LIO_LC/FAST_LIO/config/ouster64_mulran.yaml`) with the lidar topic name in your bag file.

 <img src="images/config_info.png" width="712" >

- For imu-lidar compatibility, extrinsic matrices from calibration must be changed.

<p> <img src="images/extrinsic.png" alt="Extrinsic Matrices"></p>

- To enable auto-save, `pcd_save_enable` must be `1` from the launch file (`workspace/src/FAST_LIO_LC/FAST_LIO/launch/mapping_ouster64_mulran.launch`).

### 3) Run

<!-- cspell: ignore aloam fastlio -->

- For Ouster OS1-64

      # open new terminal: run FAST-LIO
      roslaunch fast_lio mapping_ouster64.launch

      # open the other terminal tab: run SC-PGO
      roslaunch aloam_velodyne fastlio_ouster64.launch

      # play bag file in the other terminal
      rosbag play RECORDED_BAG.bag --clock

<p> <img src="images/launch.png" width="712" alt="launch"></p>

## Example Result

<p> <img src="images/fast-lio-lc-example1.png" width="712" alt="example_results1"></p>
<p> <img src="images/fast-lio-lc-example2.png" width="712" alt="example_results2"></p>

## Other Examples

<p> <img src="images/fast-lio-lc-output.png" width="712" alt="example_results"></p>

## Example dataset

Check original repository link for example dataset.

## Contact

<!-- cspell: ignore  Yanliang -->

- Maintainer: Yanliang Wang (`wyl410922@qq.com`)

## Acknowledgements

- Thanks for [FAST_LIO](https://github.com/hku-mars/FAST_LIO) authors.

<!-- In this project, the LIO module refers to FAST-LIO and the pose graph optimization refers to FAST_LIO_SLAM.

Many thanks for their work. -->
