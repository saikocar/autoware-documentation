イア・リオ・サム
IA-LIO-SAMとは何ですか?
IA_LIO_SLAM は、非構造化環境でのデータ収集用に作成されており、高精度のロボットの軌道とマッピングを実現する、スムージングとマッピングを介した強度および周囲の拡張ライダー慣性走行距離測定用のフレームワークです。
リポジトリ情報
元のリポジトリのリンク
https://github.com/minwoo0611/IA_LIO_SAM

必要なセンサー
LIDAR [ベロダイン、オースター]
IMU [9軸]
GNSS
ROS の互換性
ROS 1
依存関係
ROS (Kinetic および Melodic でテスト済み)

for ROS melodic:

sudo apt-get install -y ros-melodic-navigation
sudo apt-get install -y ros-melodic-robot-localization
sudo apt-get install -y ros-melodic-robot-state-publisher
for ROS kinetic:

sudo apt-get install -y ros-kinetic-navigation
sudo apt-get install -y ros-kinetic-robot-localization
sudo apt-get install -y ros-kinetic-robot-state-publisher
GTSAM (ジョージア工科大学のスムージングおよびマッピング ライブラリ)

wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.2.zip
cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
cd ~/Downloads/gtsam-4.0.2/
mkdir build && cd build
cmake -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF ..
sudo make install -j8
構築して実行
1) ビルド
    mkdir -p ~/catkin_ia_lio/src
    cd ~/catkin_ia_lio/src
    git clone https://github.com/minwoo0611/IA_LIO_SAM
    cd ..
    catkin_make
2) パラメータの設定
リポジトリをダウンロードした後、構成ファイルのトピックとセンサーの設定を変更します ( workspace/src/IA_LIO_SAM/config/params.yaml)

imu-lidar との互換性を確保するには、キャリブレーションの外部行列を変更する必要があります。

外部行列

自動保存を有効にするには、ファイル ( )に がsavePCD必要です。trueparams.yamlworkspace/src/IA_LIO_SAM/config/params.yaml
3) 走る
  # open new terminal: run IA_LIO
  source devel/setup.bash
  roslaunch lio_sam mapping_ouster64.launch

  # play bag file in the other terminal
  rosbag play RECORDED_BAG.bag --clock
データセットのサンプル画像
描画 描画 描画

データセットの例
データセットの例については、元のリポジトリのリンクを確認してください。

接触
メンテナ: Kevin Jung ( GitHub: minwoo0611)
紙
このコードを使用する場合は、IA-LIO-SAM(./config/doc/KRS-2021-17.pdf) を引用していただきありがとうございます。

コードの一部はLIO-SAM (IROS-2020)から適応されています。

@inproceedings{legoloam2018shan,
  title={LeGO-LOAM: Lightweight and Ground-Optimized Lidar Odometry and Mapping on Variable Terrain},
  author={Shan, Tixiao and Englot, Brendan},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={4758-4765},
  year={2018},
  organization={IEEE}
}
謝辞
IA-LIO-SAM は、LIO-SAM (T. Shan、B. Englot、D. Meyers、W. Wang、C. Ratti、および D. Rus) に基づいています。LIO-SAM: 平滑化および平滑化による密結合ライダー慣性走行距離測定マッピング）。
# IA-LIO-SAM

## What is IA-LIO-SAM?

- IA_LIO_SLAM is created for data acquisition in unstructured environment and it is a framework for Intensity and Ambient Enhanced Lidar Inertial Odometry via Smoothing and Mapping that achieves highly accurate robot trajectories and mapping.

## Repository Information

### Original Repository link

[https://github.com/minwoo0611/IA_LIO_SAM](https://github.com/minwoo0611/IA_LIO_SAM)

### Required Sensors

- LIDAR [Velodyne, Ouster]
- IMU [9-AXIS]
- GNSS

### ROS Compatibility

- ROS 1

### Dependencies

- [ROS](http://wiki.ros.org/ROS/Installation) (tested with Kinetic and Melodic)

  - `for ROS melodic:`

    ```bash
    sudo apt-get install -y ros-melodic-navigation
    sudo apt-get install -y ros-melodic-robot-localization
    sudo apt-get install -y ros-melodic-robot-state-publisher
    ```

  - `for ROS kinetic:`

    ```bash
    sudo apt-get install -y ros-kinetic-navigation
    sudo apt-get install -y ros-kinetic-robot-localization
    sudo apt-get install -y ros-kinetic-robot-state-publisher
    ```

- [GTSAM](https://github.com/borglab/gtsam/releases) (Georgia Tech Smoothing and Mapping library)
  <!-- cspell: ignore DGTSAM -->
  ```bash
  wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.2.zip
  cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
  cd ~/Downloads/gtsam-4.0.2/
  mkdir build && cd build
  cmake -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF ..
  sudo make install -j8
  ```

## Build & Run

### 1) Build

```bash
    mkdir -p ~/catkin_ia_lio/src
    cd ~/catkin_ia_lio/src
    git clone https://github.com/minwoo0611/IA_LIO_SAM
    cd ..
    catkin_make
```

### 2) Set parameters

- After downloading the repository, change topic and sensor settings on the config file (`workspace/src/IA_LIO_SAM/config/params.yaml`)

- For imu-lidar compatibility, extrinsic matrices from calibration must be changed.

<p> <img src="images/extrinsic.png"  alt="Extrinsic Matrices"></p>

- To enable autosave, `savePCD` must be `true` on the `params.yaml` file (`workspace/src/IA_LIO_SAM/config/params.yaml`).

### 3) Run

      # open new terminal: run IA_LIO
      source devel/setup.bash
      roslaunch lio_sam mapping_ouster64.launch

      # play bag file in the other terminal
      rosbag play RECORDED_BAG.bag --clock

## Sample dataset images

<p>
    <img src="images/Seq2.png" alt="drawing" width="712pix"/>
    <img src="images/Sejong_tunnel_data_lio_ai_lio.png" alt="drawing" width="712pix"/>
    <img src="images/Sejong_tunnel_data.png" alt="drawing"width="712pix"/>
</p>

## Example dataset

Check original repo link for example dataset.

## Contact

<!-- cspell: ignore minwoo -->

- Maintainer: Kevin Jung (`GitHub: minwoo0611`)

## Paper

Thank you for citing IA-LIO-SAM(./config/doc/KRS-2021-17.pdf) if you use any of this code.

Part of the code is adapted from [LIO-SAM (IROS-2020)](https://github.com/TixiaoShan/LIO-SAM).

```bash
@inproceedings{legoloam2018shan,
  title={LeGO-LOAM: Lightweight and Ground-Optimized Lidar Odometry and Mapping on Variable Terrain},
  author={Shan, Tixiao and Englot, Brendan},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={4758-4765},
  year={2018},
  organization={IEEE}
}
```

## Acknowledgements

<!-- cspell: ignore Englot  Ratti -->

- IA-LIO-SAM is based on LIO-SAM (T. Shan, B. Englot, D. Meyers, W. Wang, C. Ratti, and D. Rus. LIO-SAM: Tightly-coupled Lidar Inertial Odometry via Smoothing and Mapping).
