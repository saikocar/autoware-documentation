レゴロームボール
レゴロームボールとは何ですか?
LeGO-LOAM-BOR は、コードの品質を改善し、読みやすく一貫性を高めた LeGO-LOAM の改良版です。また、プロセスをマルチスレッドアプローチに変換することでパフォーマンスが向上します。
リポジトリ情報
元のリポジトリのリンク
https://github.com/facontidavide/LeGO-LOAM-BOR

必要なセンサー
ライダー[VLP-16]
IMU [9軸]
ROS の互換性
ROS 1
依存関係
ROS Melodic ROS のインストール
PCL PCL のインストール
Gtsam GTSAM のインストール
wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
cd ~/Downloads/gtsam-4.0.0-alpha2/
mkdir build && cd build
cmake ..
sudo make install
構築して実行
1) ビルド
cd ~/catkin_ws/src
git clone https://github.com/facontidavide/LeGO-LOAM-BOR.git
cd ..
catkin_make
2) パラメータの設定
パラメータをオンに設定しますLeGo-LOAM/loam_config.yaml
3) 走る
source devel/setup.bash
roslaunch lego_loam_bor run.launch rosbag:=/path/to/your/rosbag lidar_topic:=/velodyne_points
結果の例






レゴロームを引用
このコードのいずれかを使用する場合は、LeGO-LOAM論文を引用していただきありがとうございます。

@inproceedings{legoloam2018,
  title={LeGO-LOAM: Lightweight and Ground-Optimized Lidar Odometry and Mapping on Variable Terrain},
  author={Tixiao Shan and Brendan Englot},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={4758-4765},
  year={2018},
  organization={IEEE}
}
# LeGO-LOAM-BOR

## What is LeGO-LOAM-BOR?

- LeGO-LOAM-BOR is improved version of the LeGO-LOAM by improving quality of the code, making it more readable and consistent. Also, performance is improved by converting processes to multi-threaded approach.

## Repository Information

### Original Repository link

[https://github.com/facontidavide/LeGO-LOAM-BOR](https://github.com/facontidavide/LeGO-LOAM-BOR)

### Required Sensors

- LIDAR [VLP-16]
- IMU [9-AXIS]

### ROS Compatibility

- ROS 1

### Dependencies

- ROS Melodic [ROS Installation](http://wiki.ros.org/ROS/Installation)
- PCL [PCL Installation](https://pointclouds.org/downloads/)
- Gtsam [GTSAM Installation](https://gtsam.org/get_started/)

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
cd ~/catkin_ws/src
git clone https://github.com/facontidavide/LeGO-LOAM-BOR.git
cd ..
catkin_make
```

### 2) Set parameters

- Set parameters on `LeGo-LOAM/loam_config.yaml`

### 3) Run

```bash
source devel/setup.bash
roslaunch lego_loam_bor run.launch rosbag:=/path/to/your/rosbag lidar_topic:=/velodyne_points
```

## Example Result

<p><img src="images/demo.png" width="712"/></p>

<p><img src="images/dataset-demo.png" width="712"/></p>

<p><img src="images/google-earth.png" width="712"/></p>

## Cite _LeGO-LOAM_

Thank you for citing our _LeGO-LOAM_ paper if you use any of this code:

```bash
@inproceedings{legoloam2018,
  title={LeGO-LOAM: Lightweight and Ground-Optimized Lidar Odometry and Mapping on Variable Terrain},
  author={Tixiao Shan and Brendan Englot},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={4758-4765},
  year={2018},
  organization={IEEE}
}
```
