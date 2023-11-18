# Rosbagリプレイシミュレーション

## 手順

1. サンプルマップをダウンロードし解凍します。

   - [マップ](https://drive.google.com/file/d/1A-8BvYRX3DhSzkAnOcGWFw5T30xTlwZI/view?usp=sharing)を手動でダウンロードすることもできます。

   ```bash
   gdown -O ~/autoware_map/ 'https://docs.google.com/uc?export=download&id=1A-8BvYRX3DhSzkAnOcGWFw5T30xTlwZI'
   unzip -d ~/autoware_map/ ~/autoware_map/sample-map-rosbag.zip
   ```

2. サンプルrosbagファイルをダウンロードし解凍します。

   - [rosbagファイル](https://drive.google.com/file/d/1VnwJx9tI3kI_cTLzP61ktuAJ1ChgygpG/view?usp=sharing)を手動でダウンロードすることもできます。

   ```bash
   gdown -O ~/autoware_map/ 'https://docs.google.com/uc?export=download&id=1VnwJx9tI3kI_cTLzP61ktuAJ1ChgygpG'
   unzip -d ~/autoware_map/ ~/autoware_map/sample-rosbag.zip
   ```

3. `~/autoware_data`フォルダとファイルを確認します。

   ```bash
   $ cd ~/autoware_data
   $ ls -C -w 30
   image_projection_based_fusion
   lidar_apollo_instance_segmentation
   lidar_centerpoint
   tensorrt_yolo
   tensorrt_yolox
   traffic_light_classifier
   traffic_light_fine_detector
   traffic_light_ssd_fine_detector
   yabloc_pose_initializer
   ```

   もし異なれば[アーティファクトの手動ダウンロード](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/artifacts)を確認してください。

### 注記

- サンプルのmapとrosbag: Copyright 2020 TIER IV, Inc.
- プライバシーの観点により、rosbagには画像データが含まれていません:
  - 信号識別機能はこのサンプルではテストできません。
  - オブジェクト検出精度は低下しています。

## rosbagリプレイシミュレーションの実行方法

1. Autowareを起動します

   ```sh
   source ~/autoware/install/setup.bash
   ros2 launch autoware_launch logging_simulator.launch.xml map_path:=$HOME/autoware_map/sample-map-rosbag vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
   ```

   `$HOME`の代わりに`~`を使用することはできないことに注意してください。

   ![after-autoware-launch](images/rosbag-replay/after-autoware-launch.png)

2. サンプルrosbagファイルを再生します。

   ```sh
   source ~/autoware/install/setup.bash
   ros2 bag play ~/autoware_map/sample-rosbag/sample.db3 -r 0.2 -s sqlite3
   ```

   ![after-rosbag-play](images/rosbag-replay/after-rosbag-play.png)

3. 自己車両に注目するためにRvizビューパネルの`Target Frame`を`viewer`から`base_link`に変更します。

   ![change-target-frame](images/rosbag-replay/change-target-frame.png)

4. `Third Person Follower`等に視点を切り替えるにはRVizビューパネルの`Type`を変更します。

   ![third-person-follower](images/rosbag-replay/third-person-follower.png)

[ビデオチュートリアルを参照する](https://drive.google.com/file/d/12D6aSC1Y3Kf7STtEPWG5RYynxKdVcPrc/view?usp=sharing)
