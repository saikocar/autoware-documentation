# 地上ライダーの校正

## 概要

[Ground-Lidar校正](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/how_to_extrinsic_ground_plane.md)方法は、車両の周囲の領域が平面として表現できるという前提に基づいて動作します。
したがって、ROS 2 バッグの記録には、できるだけ広くて平らな面を見つける必要があります。
次に、このメソッドは、点群内の地面に対応する点を
base_link の XY 平面に位置合わせする方法で
キャリブレーション変換を変更します。
これは、tf の z、ロール、およびピッチの値のみがキャリブレーションを受け、
残りの x、y、およびヨーの値は、[手動調整](../extrinsic-manual-calibration)や[マッピングベースの LIDAR-LIDAR キャリブレーション](../lidar-camera-calibration)などの
他の方法を使用してキャリブレーションする必要があることを意味します。

このキャリブレーション方法を各 LIDAR に個別に適用する必要があるため、
バッグにはキャリブレーション対象のすべての LIDAR が含まれている必要があります。

生の LIDAR トピックを含む地上 LIDAR キャリブレーション プロセス用のサンプル バッグ ファイルが必要です。
サンプル バッグ ファイルが必要です。

??? 注記"tutorial_vehicle の地上ベースのキャリブレーション プロセスの ROS 2 Bag の例"

    ```sh

    Files:             rosbag2_2023_09_05-11_23_50_0.db3
    Bag size:          3.8 GiB
    Storage id:        sqlite3
    Duration:          112.702s
    Start:             Sep  5 2023 11:23:51.105 (1693902231.105)
    End:               Sep  5 2023 11:25:43.808 (1693902343.808)
    Messages:          2256
    Topic information: Topic: /sensing/lidar/front/pointcloud_raw | Type: sensor_msgs/msg/PointCloud2 | Count: 1128 | Serialization Format: cdr
                       Topic: /sensing/lidar/top/pointcloud_raw | Type: sensor_msgs/msg/PointCloud2 | Count: 1128 | Serialization Format: cdr
    ```

## 地上ライダーのキャリブレーション

### 起動ファイルの作成

前のセクションのプロセスと同様に、独自の車両の起動ファイルを作成することから始めます:

```bash
cd <YOUR-OWN-AUTOWARE-DIRECTORY>/src/autoware/calibration_tools/sensor
cd extrinsic_calibration_manager/launch
cd <YOUR-OWN-SENSOR-KIT-NAME> # i.e. for our guide, it will ve cd tutorial_vehicle_sensor_kit which is created in manual calibration
touch ground_plane.launch.xml ground_plane_sensor_kit.launch.xml
```

`ground_plane.launch.xml`と`ground_plane_sensor_kit.launch.xml`を修正するためにTIER IV のサンプル センサー キット aip_x1 を使用します。
したがって、
これら 2 つのファイルの内容を[aip_x1](https://github.com/tier4/CalibrationTools/tree/tier4/universe/sensor/extrinsic_calibration_manager/launch/aip_x1)ら作成したファイルにコピーする必要があります。

### センサーキットに応じて起動ファイルを変更する

(オプション) vehicle_id とセンサーのモデル名を追加することから始めましょう:
 (値は重要ではありません。これらのパラメーターは起動引数によってオーバーライドされます)

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

tutorial_vehicle のファイル (ground_plane.launch.xml) の最終バージョンは次のようになります:

??? 注記"チュートリアル車両のground_plane.launch.xmlファイルサンプル"

    ```xml
    <launch>
      <arg name="vehicle_id" default="tutorial_vehicle"/>
      <let name="sensor_model" value="tutorial_vehicle_sensor_kit"/>

      <group>
        <push-ros-namespace namespace="sensor_kit"/>
        <include file="$(find-pkg-share extrinsic_calibration_manager)/launch/$(var sensor_model)/ground_plane_sensor_kit.launch.xml">
          <arg name="vehicle_id" value="$(var vehicle_id)"/>
        </include>
      </group>
    </launch>

    ```

ground_plane.launch.xmlファイルが完成したら、
独自のセンサー モデルに ground_plane_sensor_kit.launch.xml を実装する準備が整います。

オプションで (これらのパラメータは起動引数によってオーバーライドされることを忘れないでください)、
次のXMLスニペットのようにsensor_kitとvehicle_idを`ground_plane.launch.xml`として変更できます:
(rviz 構成をビデオとして保存した後、
ページの最後にあるrviz_profile パスを変更できます。）

```diff
+ <?xml version="1.0" encoding="UTF-8"?>
+ <launch>
-   <arg name="vehicle_id" default="default"/>
+   <arg name="vehicle_id" default="<YOUR_VEHICLE_ID>"/>
-   <arg name="sensor_model" default="aip_x1"/>
+   <let name="sensor_model" value="<YOUR_SENSOR_KIT_NAME>"/>
    <let name="base_frame" value="base_link"/>
    <let name="parent_frame" value="sensor_kit_base_link"/>

```

地上LIDARキャリブレーションプロセスのために前にrviz設定ファイルを保存した場合:

```diff
- <let name="rviz_profile" value="$(find-pkg-share extrinsic_ground_plane_calibrator)/rviz/velodyne_top.rviz"/>
+ <let name="rviz_profile" value="$(find-pkg-share extrinsic_ground_plane_calibrator)/rviz/<YOUR-RVIZ-CONFIG>.rviz"/>
```

次に、すべてのセンサー フレームを子フレームとして extrinsic_calibration_manager に追加します:

```diff
    <!-- extrinsic_calibration_manager -->
-   <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
-     <param name="parent_frame" value="$(var parent_frame)"/>
-     <param name="child_frames" value="
-     [velodyne_top_base_link,
-     livox_front_left_base_link,
-     livox_front_center_base_link,
-     livox_front_right_base_link]"/>
-   </node>
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

tutorial_vehicleには2つのLIDARセンサー(rs_helios_topとrs_bpearl_front)があるため、
次のようになります:

??? 注記"つまり、tutorial_vehicle の extrinsic_calibration_manager child_frames"

    ```xml
    +   <!-- extrinsic_calibration_manager -->
    +   <node pkg="extrinsic_calibration_manager" exec="extrinsic_calibration_manager" name="extrinsic_calibration_manager" output="screen">
    +     <param name="parent_frame" value="$(var parent_frame)"/>
    +     <!-- add your sensor frames here -->
    +     <param name="child_frames" value="
    +     [rs_helios_top_base_link,
    +     rs_bpearl_front_base_link]"/>
    +   </node>
    ```

その後、地上ベースのキャリブレーターにLIDARセンサー構成を追加します。
そのために、次の行を`ground_plane_sensor_kit.launch.xml`ファイルに追加します:

```diff
-  <group>
-    <include file="$(find-pkg-share extrinsic_ground_plane_calibrator)/launch/calibrator.launch.xml">
-      <arg name="ns" value="$(var parent_frame)/velodyne_top_base_link"/>
-      <arg name="base_frame" value="$(var base_frame)"/>
-      <arg name="parent_frame" value="$(var parent_frame)"/>
-      <arg name="child_frame" value="velodyne_top_base_link"/>
-      <arg name="pointcloud_topic" value="/sensing/lidar/top/pointcloud_raw"/>
-    </include>
-  </group>
+  <group>
+   <include file="$(find-pkg-share extrinsic_ground_plane_calibrator)/launch/calibrator.launch.xml">
+     <arg name="ns" value="$(var parent_frame)/YOUR_SENSOR_BASE_LINK"/>
+     <arg name="base_frame" value="$(var base_frame)"/>
+     <arg name="parent_frame" value="$(var parent_frame)"/>
+     <arg name="child_frame" value="YOUR_SENSOR_BASE_LINK"/>
+     <arg name="pointcloud_topic" value="<YOUR_SENSOR_TOPIC_NAME>"/>
+   </include>
+ </group>
+  ...
+  ...
+  ...
+  ...
+  ...
+
```

??? 注記"つまり、tutorial_vehicleのLIDARごとにcalibrator.launch.xml を起動します"

    ```xml
      <!-- rs_helios_top_base_link: extrinsic_ground_plane_calibrator -->
      <group>
        <include file="$(find-pkg-share extrinsic_ground_plane_calibrator)/launch/calibrator.launch.xml">
          <arg name="ns" value="$(var parent_frame)/rs_helios_top_base_link"/>
          <arg name="base_frame" value="$(var base_frame)"/>
          <arg name="parent_frame" value="$(var parent_frame)"/>
          <arg name="child_frame" value="rs_helios_top_base_link"/>
          <arg name="pointcloud_topic" value="/sensing/lidar/top/pointcloud_raw"/>
        </include>
      </group>

      <!-- rs_bpearl_front_base_link: extrinsic_ground_plane_calibrator -->
      <group>
        <include file="$(find-pkg-share extrinsic_ground_plane_calibrator)/launch/calibrator.launch.xml">
          <arg name="ns" value="$(var parent_frame)/rs_bpearl_front_base_link"/>
          <arg name="base_frame" value="$(var base_frame)"/>
          <arg name="parent_frame" value="$(var parent_frame)"/>
          <arg name="child_frame" value="rs_bpearl_front_base_link"/>
          <arg name="pointcloud_topic" value="/sensing/lidar/front/pointcloud_raw"/>
        </include>
      </group>

      <node pkg="rviz2" exec="rviz2" name="rviz2" output="screen" args="-d $(var rviz_profile)" if="$(var calibration_rviz)"/>
    </launch>

    ```

tutorial_vehicle の ground_plane_sensor_kit.launch.xml 起動ファイルは次のようになります:

??? 注記 "tutorial_vehicle の[`ground_plane_sensor_kit.launch.xml`](https://github.com/leo-drive/tutorial_vehicle_calibration_tools/blob/tutorial_vehicle/sensor/extrinsic_calibration_manager/launch/tutorial_vehicle_sensor_kit/ground_plane_sensor_kit.launch.xml)のサンプル"

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <launch>
    <arg name="vehicle_id" default="tutorial_vehicle"/>
    <let name="sensor_model" value="tutorial_vehicle_sensor_kit"/>
    <let name="base_frame" value="base_link"/>
    <let name="parent_frame" value="sensor_kit_base_link"/>
    <let name="rviz_profile" value="$(find-pkg-share extrinsic_ground_plane_calibrator)/rviz/velodyne_top.rviz"/>
    <arg name="calibration_rviz" default="true"/>

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
        <param name="child_frames" value="
        [rs_helios_top_base_link,
        rs_bpearl_front_base_link]"/>
      </node>

      <!-- rs_helios_top_base_link: extrinsic_ground_plane_calibrator -->
      <group>
        <include file="$(find-pkg-share extrinsic_ground_plane_calibrator)/launch/calibrator.launch.xml">
          <arg name="ns" value="$(var parent_frame)/rs_helios_top_base_link"/>
          <arg name="base_frame" value="$(var base_frame)"/>
          <arg name="parent_frame" value="$(var parent_frame)"/>
          <arg name="child_frame" value="rs_helios_top_base_link"/>
          <arg name="pointcloud_topic" value="/sensing/lidar/top/pointcloud_raw"/>
        </include>
      </group>

      <!-- rs_bpearl_front_base_link: extrinsic_ground_plane_calibrator -->
      <group>
        <include file="$(find-pkg-share extrinsic_ground_plane_calibrator)/launch/calibrator.launch.xml">
          <arg name="ns" value="$(var parent_frame)/rs_bpearl_front_base_link"/>
          <arg name="base_frame" value="$(var base_frame)"/>
          <arg name="parent_frame" value="$(var parent_frame)"/>
          <arg name="child_frame" value="rs_bpearl_front_base_link"/>
          <arg name="pointcloud_topic" value="/sensing/lidar/front/pointcloud_raw"/>
        </include>
      </group>

      <node pkg="rviz2" exec="rviz2" name="rviz2" output="screen" args="-d $(var rviz_profile)" if="$(var calibration_rviz)"/>
    </launch>

    ```

### 外部地表面キャリブレータを使用した地表面 LIDAR キャリブレーション プロセス

mapping_based.launch.xml および Mapping_based_sensor_kit.launch.xml を完了したら、独自のセンサー キット用のファイルを起動します;
これで、LIDAR を調整する準備が整いました。
まず最初に、extrinsic_calibration_manager パッケージをビルドする必要があります:

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-select extrinsic_calibration_manager
```

これで、地上ベースの LIDAR-地上校正器を起動して使用する準備が整いました。

```bash
ros2 launch extrinsic_calibration_manager calibration.launch.xml mode:=ground_plane sensor_model:=<OWN-SENSOR-KIT> vehicle_model:=<OWN-VEHICLE-MODEL> vehicle_id:=<VEHICLE-ID>
```

チュートリアル車両の場合:

```bash
ros2 launch extrinsic_calibration_manager calibration.launch.xml mode:=ground_plane sensor_model:=tutorial_vehicle_sensor_kit vehicle_model:=tutorial_vehicle vehicle_id:=tutorial_vehicle
```

いくつかの設定を含む rviz2 画面を表示します。
ドキュメントの最後にあるビデオのように、
センサー情報トピック、sensor_frames、pointcloud_inlier_topics で
画面を更新する必要があります。
また、rviz2 設定を rviz ディレクトリに保存できるため、
後で`mapping_based_sensor_kit.launch.xml`を変更して使用できます。

```diff
extrinsic_mapping_based_calibrator/
   └─ rviz/
+        └─ tutorial_vehicle_sensor_kit.rviz
```

次に、ROS 2 バッグ ファイルを再生すると、キャリブレーション プロセスが開始されます:

```bash
ros2 bag play <rosbag_path> --clock -l -r 0.2 \
--remap /tf:=/null/tf /tf_static:=/null/tf_static # if tf is recorded
```

調整プロセスは自動的に行われるため、
調整プロセスが完了すると、$HOME ディレクトリに sensor_kit_calibration.yaml が表示されます。

|          グランドプレーン前 - LIDAR キャリブレーション           |             グラウンドプレーン後 - LIDAR キャリブレーション              |
| :--------------------------------------------------------: | :-------------------------------------------------------------: |
| ![before-ground-plane.png](images/before-ground-plane.png) | ![images/after-ground-plane.png](images/after-ground-plane.png) |

これは、tutorial_vehicle での地表 - LIDAR キャリブレーション プロセスをデモンストレーションするビデオです:
![type:video](https://youtube.com/embed/EqaF1fufjUc)
