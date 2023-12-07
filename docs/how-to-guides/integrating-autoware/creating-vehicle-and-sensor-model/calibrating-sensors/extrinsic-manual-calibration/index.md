すべてのセンサーの手動校正
概要
このセクションでは、センサーの外部キャリブレーションに外部手動キャリブレーションを使用します。このプロセスの後、最終的に使用するための正確なキャリブレーション結果は得られませんが、他のツールの初期キャリブレーションは行われます。たとえば、ライダーとライダーまたはカメラとライダーのキャリブレーション フェーズでは、正確で適切なキャリブレーション結果を得るために初期キャリブレーションが必要になります。

キャリブレーション プロセスには、生の LIDAR トピックとカメラ トピックを含むサンプル バッグ ファイルが必要です。以下に、キャリブレーションに使用されるバッグ ファイルの例を示します。

??? 注「ROS 2 バッグの校正プロセスの例」

```sh

Files:             rosbag2_2023_09_06-13_43_54_0.db3
Bag size:          18.3 GiB
Storage id:        sqlite3
Duration:          169.12s
Start:             Sep  6 2023 13:43:54.902 (1693997034.902)
End:               Sep  6 2023 13:46:43.914 (1693997203.914)
Messages:          8504
Topic information: Topic: /sensing/lidar/top/pointcloud_raw | Type: sensor_msgs/msg/PointCloud2 | Count: 1691 | Serialization Format: cdr
                   Topic: /sensing/lidar/front/pointcloud_raw | Type: sensor_msgs/msg/PointCloud2 | Count: 1691 | Serialization Format: cdr
                   Topic: /sensing/camera/camera0/image_rect | Type: sensor_msgs/msg/Image | Count: 2561 | Serialization Format: cdr
                   Topic: /sensing/camera/camera0/camera_info | Type: sensor_msgs/msg/CameraInfo | Count: 2561 | Serialization Format: cdr
```
外部の手動ベースの校正
起動ファイルの作成
まず、extrinsic_calibration_managerパッケージの起動ファイルの作成から始めます。

cd <YOUR-OWN-AUTOWARE-DIRECTORY>/src/autoware/calibration_tools/sensor
cd extrinsic_calibration_manager/launch
mkdir <YOUR-OWN-SENSOR-KIT-NAME> # i.e. for our guide, it will ve mkdir tutorial_vehicle_sensor_kit
cd <YOUR-OWN-SENSOR-KIT-NAME> # i.e. for our guide, it will ve cd tutorial_vehicle_sensor_kit
touch manual.launch.xml manual_sensor_kit.launch.xml manual_sensors.launch.xml
これらを変更し、manual.launch.xmlTIER IV のサンプル センサー キット aip_x1 を使用します。したがって、これら 3 つのファイルの内容をaip_x1から作成したファイルにコピーする必要があります。manual_sensors.launch.xmlmanual_sensor_kit.launch.xml

センサーキットに応じて起動ファイルを変更する
それで、 の変更を開始できますmanual.launch.xml。お好みのテキスト エディター (コード、gedit など) でこのファイルを開いてください。

(オプション) vehicle_id とセンサーのモデル名を追加することから始めましょう: (値は重要ではありません。これらのパラメーターは起動引数によってオーバーライドされます)

  <arg name="vehicle_id" default="default"/>

  <let name="sensor_model" value="aip_x1"/>
+ <?xml version="1.0" encoding="UTF-8"?>
+ <launch>
-   <arg name="vehicle_id" default="default"/>
+   <arg name="vehicle_id" default="<YOUR_VEHICLE_ID>"/>
+
-   <arg name="sensor_model" default="aip_x1"/>
+   <let name="sensor_model" value="<YOUR_SENSOR_KIT_NAME>"/>
tutorial_vehicle のファイル (manual.launch.xml) の最終バージョンは次のようになります。

??? 注「チュートリアル車両のサンプルマニュアル.launch.xml ファイル」

```xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
    <arg name="vehicle_id" default="tutorial_vehicle"/>

    <let name="sensor_model" value="tutorial_vehicle_sensor_kit"/>

    <group>
        <push-ros-namespace namespace="sensor_kit"/>
        <include file="$(find-pkg-share extrinsic_calibration_manager)/launch/$(var sensor_model)/manual_sensor_kit.launch.xml">
            <arg name="vehicle_id" value="$(var vehicle_id)"/>
        </include>
    </group>

    <group>
        <push-ros-namespace namespace="sensors"/>
        <include file="$(find-pkg-share extrinsic_calibration_manager)/launch/$(var sensor_model)/manual_sensors.launch.xml">
          <arg name="vehicle_id" value="$(var vehicle_id)"/>
        </include>
    </group>

</launch>

```
Manual.launch.xml ファイルが完成したら、独自のセンサー モデルのsensor_kit_calibration.yamlに Manual_sensor_kit.launch.xml を実装する準備が整います。

オプションで、この XML スニペットに対して sensor_model と vehicle_id を変更することもできます。

...
  <arg name="vehicle_id" default="default"/>

  <let name="sensor_model" value="aip_x1"/>
+ <?xml version="1.0" encoding="UTF-8"?>
+ <launch>
-   <arg name="vehicle_id" default="default"/>
+   <arg name="vehicle_id" default="<YOUR_VEHICLE_ID>"/>
+
-   <arg name="sensor_model" default="aip_x1"/>
+   <let name="sensor_model" value="<YOUR_SENSOR_KIT_NAME>"/>
...
次に、すべてのセンサー フレームを子フレームとして extrinsic_calibration_manager に追加します。

   <!-- extrinsic_calibration_manager -->
-  <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
-    <param name="parent_frame" value="$(var parent_frame)"/>
-    <param name="child_frames" value="
-    [velodyne_top_base_link,
-    livox_front_left_base_link,
-    livox_front_center_base_link,
-    livox_front_right_base_link]"/>
-  </node>
+   <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
+     <param name="parent_frame" value="$(var parent_frame)"/>
+     <!-- add your sensor frames here -->
+     <param name="child_frames" value="
+     [<YOUE_SENSOR_BASE_LINK>,
+     YOUE_SENSOR_BASE_LINK,
+     YOUE_SENSOR_BASE_LINK,
+     YOUE_SENSOR_BASE_LINK
+     ...]"/>
+   </node>
tutorial_vehicle には 4 つのセンサー (2 つの LIDAR、1 つのカメラ、1 つの GNSS/INS) があるため、次のようになります。

??? 注「つまり、tutorial_vehicle の extrinsic_calibration_manager child_frames」

```xml
+   <!-- extrinsic_calibration_manager -->
+   <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
+     <param name="parent_frame" value="$(var parent_frame)"/>
+     <!-- add your sensor frames here -->
+     <param name="child_frames" value="
+     [rs_helios_top_base_link,
+     rs_bpearl_front_base_link,
+     camera0/camera_link,
+     gnss_link]"/>
+   </node>
```
最後に、センサーのフレームごとに手動キャリブレーターを起動します。calibrator.launch.xml 起動ファイル引数の名前空間 (ns) と child_frame 引数を更新してください。

-  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
-    <arg name="ns" value="$(var parent_frame)/velodyne_top_base_link"/>
-    <arg name="parent_frame" value="$(var parent_frame)"/>
-    <arg name="child_frame" value="velodyne_top_base_link"/>
-  </include>
+  <!-- extrinsic_manual_calibrator -->
+  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
+    <arg name="ns" value="$(var parent_frame)/<YOUR_SENSOR_BASE_LINK>"/>
+    <arg name="parent_frame" value="$(var parent_frame)"/>
+    <arg name="child_frame" value="<YOUR_SENSOR_BASE_LINK>""/>
+  </include>
+
+  ...
+  ...
+  ...
+  ...
+  ...
+
??? 注「つまり、各tutorial_vehicleのセンサーキットのcalibrator.launch.xml」

```xml
+  <!-- extrinsic_manual_calibrator -->
+  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
+    <arg name="ns" value="$(var parent_frame)/rs_helios_top_base_link"/>
+    <arg name="parent_frame" value="$(var parent_frame)"/>
+    <arg name="child_frame" value="rs_helios_top_base_link"/>
+  </include>
+
+  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
+    <arg name="ns" value="$(var parent_frame)/rs_bpearl_front_base_link"/>
+    <arg name="parent_frame" value="$(var parent_frame)"/>
+    <arg name="child_frame" value="rs_bpearl_front_base_link"/>
+  </include>
+
+  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
+    <arg name="ns" value="$(var parent_frame)/camera0/camera_link"/>
+    <arg name="parent_frame" value="$(var parent_frame)"/>
+    <arg name="child_frame" value="camera0/camera_link"/>
+  </include>
+
+  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
+    <arg name="ns" value="$(var parent_frame)/gnss_link"/>
+    <arg name="parent_frame" value="$(var parent_frame)"/>
+    <arg name="child_frame" value="gnss_link"/>
+  </include>
+ </launch>
```
tutorial_vehicle の Manual_sensor_kit.launch.xml の最終バージョンは次のようになります。

??? 注「manual_sensor_kit.launch.xmltutorial_vehicle のサンプル」

```xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
    <arg name="vehicle_id" default="tutorial_vehicle"/> <!-- You can update with your own vehicle_id -->

    <let name="sensor_model" value="tutorial_vehicle_sensor_kit"/> <!-- You can update with your own sensor model -->
    <let name="parent_frame" value="sensor_kit_base_link"/>

    <!-- extrinsic_calibration_client -->
    <arg name="src_yaml" default="$(find-pkg-share individual_params)/config/$(var vehicle_id)/$(var sensor_model)/sensor_kit_calibration.yaml"/>
    <arg name="dst_yaml" default="$(env HOME)/sensor_kit_calibration.yaml"/>

    <node pkg="extrinsic_calibration_client" exec="extrinsic_calibration_client" name="extrinsic_calibration_client" output="screen">
        <param name="src_path" value="$(var src_yaml)"/>
        <param name="dst_path" value="$(var dst_yaml)"/>
    </node>

    <!-- extrinsic_calibration_manager -->
    <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
        <param name="parent_frame" value="$(var parent_frame)"/>
        <!-- Please Update with your own sensor frames -->
        <param name="child_frames" value="
    [rs_helios_top_base_link,
    rs_bpearl_front_base_link,
    camera0/camera_link,
    gnss_link]"/>
    </node>

    <!-- extrinsic_manual_calibrator -->
    <!-- Please create a launch for all sensors that you used. -->
    <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
        <arg name="ns" value="$(var parent_frame)/rs_helios_top_base_link"/>
        <arg name="parent_frame" value="$(var parent_frame)"/>
        <arg name="child_frame" value="rs_helios_top_base_link"/>
    </include>

    <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
        <arg name="ns" value="$(var parent_frame)/rs_bpearl_front_base_link"/>
        <arg name="parent_frame" value="$(var parent_frame)"/>
        <arg name="child_frame" value="rs_bpearl_front_base_link"/>
    </include>

    <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
        <arg name="ns" value="$(var parent_frame)/camera0/camera_link"/>
        <arg name="parent_frame" value="$(var parent_frame)"/>
        <arg name="child_frame" value="camera0/camera_link"/>
    </include>

    <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
        <arg name="ns" value="$(var parent_frame)/gnss_link"/>
        <arg name="parent_frame" value="$(var parent_frame)"/>
        <arg name="child_frame" value="gnss_link"/>
    </include>
</launch>
```
変更したsensors_calibration.yamlmanual_sensors.launch.xmlファイルに従ってファイルを更新できます。センサーをtutorial_vehicleのbase_linkに関して直接調整するつもりはないので、このファイルは変更しません。

外部手動校正器によるセンサーの校正
extrinsic_calibration_manager パッケージの Manual.launch.xml および Manual_sensor_kit.launch XML ファイルが完成したら、パッケージをビルドする必要があります。

colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-select extrinsic_calibration_manager
これで、手動キャリブレーターを起動して使用する準備が整いました。

ros2 launch extrinsic_calibration_manager calibration.launch.xml mode:=manual sensor_model:=<OWN-SENSOR-KIT> vehicle_model:=<OWN-VEHICLE-MODEL> vehicle_id:=<VEHICLE-ID>
チュートリアル車両の場合:

ros2 launch extrinsic_calibration_manager calibration.launch.xml mode:=manual sensor_model:=tutorial_vehicle_sensor_kit vehicle_model:=tutorial_vehicle vehicle_id:=tutorial_vehicle
次に、ROS 2 バッグ ファイルを再生します。

ros2 bag play <rosbag_path> --clock -l -r 0.2 \
--remap /tf:=/null/tf /tf_static:=/null/tf_static # if tf is recorded
手動の rqt_reconfigure ウィンドウが表示され、センサーの rviz2 結果に従って手動でキャリブレーションを更新します。

ボタンを押してRefreshからExpand Allボタンを押します。tutorial_vehicle のフレームは次のようになります。
フォーク-autoware_repository.png

tf_x, tf_y, tf_z, tf_roll, tf_pitch and tf_yawFilter エリアにターゲット フレーム名 (つまり、front、helios など) を書き込み、tuneable_static_tf_broadcaster_node を選択すると、 RQT パネルで値を調整できます。
手動調整が終了したら、次のコマンドを使用してキャリブレーション結果を保存できます。
ros2 topic pub /done std_msgs/Bool "data: true"
その後、$HOME/*.yaml で出力ファイルを確認できます。
!!! 警告

The initial calibration process can be important before the using other calibrations. We will look into the lidar-lidar calibration
and camera-lidar calibration. At this point, there is hard to calibrate two sensors with exactly same frame, so you should find
approximately (it not must be perfect) calibration pairs between sensors.
これは、tutorial_vehicle での手動調整プロセスをデモンストレーションするビデオです。 タイプ:ビデオ


# Manual calibration for all sensors

## Overview

In this section, we will use [Extrinsic Manual Calibration](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/how_to_extrinsic_manual.md)
for extrinsic calibration of our sensors.
After this process, we won't get accurate calibration results for final use,
but we will have an initial calibration for other tools.
For example, in the lidar-lidar or camera-lidar calibration phase,
we will need initial calibration for getting accurate and successful calibration results.

We need a sample bag file for the calibration process
which includes raw lidar topics and camera topics.
The following shows an example of a bag file used for calibration:

??? note "ROS 2 Bag example of our calibration process"

    ```sh

    Files:             rosbag2_2023_09_06-13_43_54_0.db3
    Bag size:          18.3 GiB
    Storage id:        sqlite3
    Duration:          169.12s
    Start:             Sep  6 2023 13:43:54.902 (1693997034.902)
    End:               Sep  6 2023 13:46:43.914 (1693997203.914)
    Messages:          8504
    Topic information: Topic: /sensing/lidar/top/pointcloud_raw | Type: sensor_msgs/msg/PointCloud2 | Count: 1691 | Serialization Format: cdr
                       Topic: /sensing/lidar/front/pointcloud_raw | Type: sensor_msgs/msg/PointCloud2 | Count: 1691 | Serialization Format: cdr
                       Topic: /sensing/camera/camera0/image_rect | Type: sensor_msgs/msg/Image | Count: 2561 | Serialization Format: cdr
                       Topic: /sensing/camera/camera0/camera_info | Type: sensor_msgs/msg/CameraInfo | Count: 2561 | Serialization Format: cdr
    ```

## Extrinsic Manual-Based Calibration

### Creating launch files

First of all, we will start with creating launch file for `extrinsic_calibration_manager` package:

```bash
cd <YOUR-OWN-AUTOWARE-DIRECTORY>/src/autoware/calibration_tools/sensor
cd extrinsic_calibration_manager/launch
mkdir <YOUR-OWN-SENSOR-KIT-NAME> # i.e. for our guide, it will ve mkdir tutorial_vehicle_sensor_kit
cd <YOUR-OWN-SENSOR-KIT-NAME> # i.e. for our guide, it will ve cd tutorial_vehicle_sensor_kit
touch manual.launch.xml manual_sensor_kit.launch.xml manual_sensors.launch.xml
```

We will be modifying these `manual.launch.xml`, `manual_sensors.launch.xml` and `manual_sensor_kit.launch.xml` by using TIER IV's sample sensor kit aip_x1.
So,
you should copy the contents of these three files from [aip_x1](https://github.com/tier4/CalibrationTools/tree/tier4/universe/sensor/extrinsic_calibration_manager/launch/aip_x1) to your created files.

### Modifying launch files according to your sensor kit

So, we can start modifying `manual.launch.xml`,
please open this file on a text editor which will you prefer (code, gedit etc.).

(Optionally) Let's start with adding vehicle_id and sensor model names:
(Values are not important. These parameters will be overridden by launch arguments)

```diff
  <arg name="vehicle_id" default="default"/>

  <let name="sensor_model" value="aip_x1"/>
+ <?xml version="1.0" encoding="UTF-8"?>
+ <launch>
-   <arg name="vehicle_id" default="default"/>
+   <arg name="vehicle_id" default="<YOUR_VEHICLE_ID>"/>
+
-   <arg name="sensor_model" default="aip_x1"/>
+   <let name="sensor_model" value="<YOUR_SENSOR_KIT_NAME>"/>
```

The final version of the file (manual.launch.xml) for tutorial_vehicle should be like this:

??? note "Sample manual.launch.xml file for tutorial vehicle"

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <launch>
        <arg name="vehicle_id" default="tutorial_vehicle"/>

        <let name="sensor_model" value="tutorial_vehicle_sensor_kit"/>

        <group>
            <push-ros-namespace namespace="sensor_kit"/>
            <include file="$(find-pkg-share extrinsic_calibration_manager)/launch/$(var sensor_model)/manual_sensor_kit.launch.xml">
                <arg name="vehicle_id" value="$(var vehicle_id)"/>
            </include>
        </group>

        <group>
            <push-ros-namespace namespace="sensors"/>
            <include file="$(find-pkg-share extrinsic_calibration_manager)/launch/$(var sensor_model)/manual_sensors.launch.xml">
              <arg name="vehicle_id" value="$(var vehicle_id)"/>
            </include>
        </group>

    </launch>

    ```

After the completing of manual.launch.xml file,
we will be ready to implement manual_sensor_kit.launch.xml for the own sensor model's [sensor_kit_calibration.yaml](https://github.com/autowarefoundation/sample_sensor_kit_launch/blob/main/sample_sensor_kit_description/config/sensor_kit_calibration.yaml):

Optionally, you can modify sensor_model and vehicle_id over this xml snippet as well:

```diff
...
  <arg name="vehicle_id" default="default"/>

  <let name="sensor_model" value="aip_x1"/>
+ <?xml version="1.0" encoding="UTF-8"?>
+ <launch>
-   <arg name="vehicle_id" default="default"/>
+   <arg name="vehicle_id" default="<YOUR_VEHICLE_ID>"/>
+
-   <arg name="sensor_model" default="aip_x1"/>
+   <let name="sensor_model" value="<YOUR_SENSOR_KIT_NAME>"/>
...
```

Then, we will add all our sensor frames on extrinsic_calibration_manager as child frames:

```diff
   <!-- extrinsic_calibration_manager -->
-  <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
-    <param name="parent_frame" value="$(var parent_frame)"/>
-    <param name="child_frames" value="
-    [velodyne_top_base_link,
-    livox_front_left_base_link,
-    livox_front_center_base_link,
-    livox_front_right_base_link]"/>
-  </node>
+   <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
+     <param name="parent_frame" value="$(var parent_frame)"/>
+     <!-- add your sensor frames here -->
+     <param name="child_frames" value="
+     [<YOUE_SENSOR_BASE_LINK>,
+     YOUE_SENSOR_BASE_LINK,
+     YOUE_SENSOR_BASE_LINK,
+     YOUE_SENSOR_BASE_LINK
+     ...]"/>
+   </node>
```

For tutorial_vehicle there are four sensors (two lidar, one camera, one gnss/ins), so it will be like this:

??? note "i.e extrinsic_calibration_manager child_frames for tutorial_vehicle"

    ```xml
    +   <!-- extrinsic_calibration_manager -->
    +   <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
    +     <param name="parent_frame" value="$(var parent_frame)"/>
    +     <!-- add your sensor frames here -->
    +     <param name="child_frames" value="
    +     [rs_helios_top_base_link,
    +     rs_bpearl_front_base_link,
    +     camera0/camera_link,
    +     gnss_link]"/>
    +   </node>
    ```

Lastly, we will launch a manual calibrator each frame for our sensors,
please update namespace (ns) and child_frame argument on calibrator.launch.xml launch file argument:

```diff
-  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
-    <arg name="ns" value="$(var parent_frame)/velodyne_top_base_link"/>
-    <arg name="parent_frame" value="$(var parent_frame)"/>
-    <arg name="child_frame" value="velodyne_top_base_link"/>
-  </include>
+  <!-- extrinsic_manual_calibrator -->
+  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
+    <arg name="ns" value="$(var parent_frame)/<YOUR_SENSOR_BASE_LINK>"/>
+    <arg name="parent_frame" value="$(var parent_frame)"/>
+    <arg name="child_frame" value="<YOUR_SENSOR_BASE_LINK>""/>
+  </include>
+
+  ...
+  ...
+  ...
+  ...
+  ...
+
```

??? note "i.e., calibrator.launch.xml for each tutorial_vehicle's sensor kit"

    ```xml
    +  <!-- extrinsic_manual_calibrator -->
    +  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
    +    <arg name="ns" value="$(var parent_frame)/rs_helios_top_base_link"/>
    +    <arg name="parent_frame" value="$(var parent_frame)"/>
    +    <arg name="child_frame" value="rs_helios_top_base_link"/>
    +  </include>
    +
    +  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
    +    <arg name="ns" value="$(var parent_frame)/rs_bpearl_front_base_link"/>
    +    <arg name="parent_frame" value="$(var parent_frame)"/>
    +    <arg name="child_frame" value="rs_bpearl_front_base_link"/>
    +  </include>
    +
    +  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
    +    <arg name="ns" value="$(var parent_frame)/camera0/camera_link"/>
    +    <arg name="parent_frame" value="$(var parent_frame)"/>
    +    <arg name="child_frame" value="camera0/camera_link"/>
    +  </include>
    +
    +  <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
    +    <arg name="ns" value="$(var parent_frame)/gnss_link"/>
    +    <arg name="parent_frame" value="$(var parent_frame)"/>
    +    <arg name="child_frame" value="gnss_link"/>
    +  </include>
    + </launch>
    ```

The final version of the manual_sensor_kit.launch.xml for tutorial_vehicle should be like this:

??? note "Sample [`manual_sensor_kit.launch.xml`](https://github.com/leo-drive/tutorial_vehicle_calibration_tools/blob/tutorial_vehicle/sensor/extrinsic_calibration_manager/launch/tutorial_vehicle_sensor_kit/manual_sensor_kit.launch.xml) for tutorial_vehicle"

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <launch>
        <arg name="vehicle_id" default="tutorial_vehicle"/> <!-- You can update with your own vehicle_id -->

        <let name="sensor_model" value="tutorial_vehicle_sensor_kit"/> <!-- You can update with your own sensor model -->
        <let name="parent_frame" value="sensor_kit_base_link"/>

        <!-- extrinsic_calibration_client -->
        <arg name="src_yaml" default="$(find-pkg-share individual_params)/config/$(var vehicle_id)/$(var sensor_model)/sensor_kit_calibration.yaml"/>
        <arg name="dst_yaml" default="$(env HOME)/sensor_kit_calibration.yaml"/>

        <node pkg="extrinsic_calibration_client" exec="extrinsic_calibration_client" name="extrinsic_calibration_client" output="screen">
            <param name="src_path" value="$(var src_yaml)"/>
            <param name="dst_path" value="$(var dst_yaml)"/>
        </node>

        <!-- extrinsic_calibration_manager -->
        <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
            <param name="parent_frame" value="$(var parent_frame)"/>
            <!-- Please Update with your own sensor frames -->
            <param name="child_frames" value="
        [rs_helios_top_base_link,
        rs_bpearl_front_base_link,
        camera0/camera_link,
        gnss_link]"/>
        </node>

        <!-- extrinsic_manual_calibrator -->
        <!-- Please create a launch for all sensors that you used. -->
        <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
            <arg name="ns" value="$(var parent_frame)/rs_helios_top_base_link"/>
            <arg name="parent_frame" value="$(var parent_frame)"/>
            <arg name="child_frame" value="rs_helios_top_base_link"/>
        </include>

        <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
            <arg name="ns" value="$(var parent_frame)/rs_bpearl_front_base_link"/>
            <arg name="parent_frame" value="$(var parent_frame)"/>
            <arg name="child_frame" value="rs_bpearl_front_base_link"/>
        </include>

        <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
            <arg name="ns" value="$(var parent_frame)/camera0/camera_link"/>
            <arg name="parent_frame" value="$(var parent_frame)"/>
            <arg name="child_frame" value="camera0/camera_link"/>
        </include>

        <include file="$(find-pkg-share extrinsic_manual_calibrator)/launch/calibrator.launch.xml">
            <arg name="ns" value="$(var parent_frame)/gnss_link"/>
            <arg name="parent_frame" value="$(var parent_frame)"/>
            <arg name="child_frame" value="gnss_link"/>
        </include>
    </launch>
    ```

You can update `manual_sensors.launch.xml` file according to your modified [sensors_calibration.yaml](https://github.com/autowarefoundation/sample_sensor_kit_launch/blob/main/sample_sensor_kit_description/config/sensors_calibration.yaml) file.
Since we will not be calibrating the sensor directly with respect to the base_link in tutorial_vehicle,
we will not change this file.

### Calibrating sensors with extrinsic manual calibrator

After the completion of manual.launch.xml and manual_sensor_kit.launch xml file for extrinsic_calibration_manager package,
we need to build package:

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-select extrinsic_calibration_manager
```

So, we are ready to launch and use manual calibrator:

```bash
ros2 launch extrinsic_calibration_manager calibration.launch.xml mode:=manual sensor_model:=<OWN-SENSOR-KIT> vehicle_model:=<OWN-VEHICLE-MODEL> vehicle_id:=<VEHICLE-ID>
```

For tutorial vehicle:

```bash
ros2 launch extrinsic_calibration_manager calibration.launch.xml mode:=manual sensor_model:=tutorial_vehicle_sensor_kit vehicle_model:=tutorial_vehicle vehicle_id:=tutorial_vehicle
```

Then play ROS 2 bag file:

```bash
ros2 bag play <rosbag_path> --clock -l -r 0.2 \
--remap /tf:=/null/tf /tf_static:=/null/tf_static # if tf is recorded
```

You will show to a manual rqt_reconfigure window,
we will update calibrations by hand according to the rviz2 results of sensors.

- Press `Refresh` button then press `Expand All` button. The frames on tutorial_vehicle should like this:

![forking-autoware_repository.png](images/manual-calibration.png)

- Please write the target frame name in Filter area (i.e., front, helios etc.) and select tunable_static_tf_broadcaster_node, then you can adjust `tf_x, tf_y, tf_z, tf_roll, tf_pitch and tf_yaw` values over RQT panel.
- If manual adjusting is finished, you can save your calibration results via this command:

```bash
ros2 topic pub /done std_msgs/Bool "data: true"
```

- Then you can check the output file in $HOME/\*.yaml.

!!! warning

    The initial calibration process can be important before the using other calibrations. We will look into the lidar-lidar calibration
    and camera-lidar calibration. At this point, there is hard to calibrate two sensors with exactly same frame, so you should find
    approximately (it not must be perfect) calibration pairs between sensors.

Here is the video for demonstrating a manual calibration process on tutorial_vehicle:
![type:video](https://youtube.com/embed/axHILP0PiaQ)
