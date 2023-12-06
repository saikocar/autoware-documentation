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

!!! info [Using Autoware Launch GUI](#using-autoware-launch-gui)

    If you prefer a graphical user interface (GUI) over the command line for launching and managing your simulations, refer to the `Using Autoware Launch GUI` section at the end of this document for a step-by-step guide.

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

## Using Autoware Launch GUI

This section provides a step-by-step guide for using the Autoware Launch GUI to launch and manage your rosbag replay simulation. offering an alternative to the command-line instructions provided in the previous section.

### Getting Started with Autoware Launch GUI

1. **Installation:** Ensure you have installed the Autoware Launch GUI. [Installation instructions](https://github.com/autowarefoundation/autoware-launch-gui#installation).

2. **Launching the GUI:** Open the Autoware Launch GUI from your applications menu.
   ![GUI_screenshot_for_launching](images/rosbag-replay/launch-gui/launch_gui_main.png)

### Launching a Logging Simulation

1. **Set Autoware Path:** In the GUI, set the path to your Autoware installation.
   ![GUI_screenshot_for_setting_Autoware_path](images/rosbag-replay/launch-gui/launch_gui_setup.png)
2. **Select Launch File:** Choose `logging_simulator.launch.xml` for the lane driving scenario.
   ![GUI screenshot for selecting launch file](images/rosbag-replay/launch-gui/selecting_launch_file.png)
3. **Customize Parameters:** Adjust parameters such as `map_path`, `vehicle_model`, and `sensor_model` as needed.

   ![GUI screenshot for customizing parameters](images/rosbag-replay/launch-gui/customizing-parameters1.png)
   ![GUI screenshot for customizing parameters](images/rosbag-replay/launch-gui/customizing-parameters2.png)

4. **Start Simulation:** Click the launch button to start the simulation and have access to all the logs.

   ![GUI screenshot for starting simulation](images/rosbag-replay/launch-gui/starting_simulation.png)

5. **Play Rosbag:** Move to the `Rosbag` tab and select the rosbag file you wish to play.

   ![GUI screenshot for selecting rosbag file](images/rosbag-replay/launch-gui/selecting_rosbag_file.png)

6. **Adjust Playback Speed:** Adjust the playback speed as needed and any other parameters you wish to customize.

   ![GUI screenshot for adjusting playback speed](images/rosbag-replay/launch-gui/adjusting_flags.png)

7. **Start Playback:** Click the play button to start the rosbag playback and have access to settings such as `pause/play`, `stop`, and `speed slider`5.

   ![GUI screenshot for starting playback](images/rosbag-replay/launch-gui/starting_playback.png)

8. **View Simulation:** Move to the `RViz` window to view the simulation.

   ![after-rosbag-play](images/rosbag-replay/after-rosbag-play.png)

9. To focus the view on the ego vehicle, change the `Target Frame` in the RViz Views panel from `viewer` to `base_link`.

   ![change-target-frame](images/rosbag-replay/change-target-frame.png)

10. To switch the view to `Third Person Follower` etc, change the `Type` in the RViz Views panel.

    ![third-person-follower](images/rosbag-replay/third-person-follower.png)
