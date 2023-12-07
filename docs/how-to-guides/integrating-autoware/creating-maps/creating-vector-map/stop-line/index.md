停止線
行動速度プランナーの停止線モジュールは、停止線の直前で停止し、停止後に運転を再開する速度を計画します。それを操作するために、lanlet2 マップに停止線属性を追加します。

停止線規制要素の作成
ポイントクラウド マップ上に停止線を作成するには、次の手順に従ってください。

追加する停止線を選択してください。
トップパネルの ボタンをクリックしますAbstraction。
Stop Lineパネルから選択します。
停止線を挿入したい領域をクリックします。
停止線作成のデモンストレーション ビデオでこれらの手順を確認できます。

タイプ:ビデオ

計画シミュレータを使用して停止線要素を作成したテスト
マップの作成が完了したら、マップを保存する必要があります。File-->をクリックしExport Lanelet2Mapsてダウンロードしてください。

ダウンロードが完了したら、lanelet2 マップと pointcloud マップを同じ場所に配置する必要があります。ディレクトリ構造は次のようになります。

+ <YOUR-MAP-DIRECTORY>/
+  ├─ pointcloud_map.pcd
+  └─ lanelet2_map.osm
.osm または .pcd マップ ファイルの名前がこれらの名前と異なる場合は、autoware.launch.xml を更新する必要があります。

  <!-- Map -->
-  <arg name="lanelet2_map_file" default="lanelet2_map.osm" description="lanelet2 map file name"/>
+  <arg name="lanelet2_map_file" default="<YOUR-LANELET-MAP-NAME>.osm" description="lanelet2 map file name"/>
-  <arg name="pointcloud_map_file" default="pointcloud_map.pcd" description="pointcloud map file name"/>
+  <arg name="pointcloud_map_file" default="<YOUR-POINTCLOUD-MAP-NAME>.pcd" description="pointcloud map file name"/>
これで、計画シミュレーターを起動する準備が整いました。

ros2 launch autoware_launch planning_simulator.launch.xml map_path:=<YOUR-MAP-FOLDER-DIR> vehicle_model:=<YOUR-VEHICLE-MODEL> sensor_model:=<YOUR-SENSOR-KIT>
チュートリアル_車両の例:

ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/Files/autoware_map/tutorial_map/ vehicle_model:=tutorial_vehicle sensor_model:=tutorial_vehicle_sensor_kit vehicle_id:=tutorial_vehicle
2D Pose Estimaterviz の ボタンをクリックするか、 を押してPポーズを与えると初期化されます。
2D Goal Poserviz のボタンをクリックするか、 を押してGゴールポイントのポーズをとります。
rviz 画面上に停止線マーカーが表示されます。
rviz の停止線マーカー:

![stop-line-test](images/stop-line-test.png){ align=center } 作成した地図上で停止線テストを行います。
次のデモビデオのように、計画シミュレーターで停止線要素を確認できます。

タイプ:ビデオ
# Stop Line

Behavior velocity planner's [stop line module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_stop_line_module/) plans velocity
to stop right before stop lines and restart driving after stopped.
In order to operate that, we will add stop line attribute to our lanelet2 map.

## Creating a stop line regulatory element

In order to create a stop line on your pointcloud map, please follow these steps:

1. Please select lanelet which stop line to be added.
2. Click `Abstraction` button on top panel.
3. Select `Stop Line` from the panel.
4. Click on the desired area for inserting stop line.

You can see these steps in the stop line creating demonstration video:

![type:video](https://youtube.com/embed/cgTSA50Yfyo)

### Testing created the stop line element with planning simulator

After the completing of creating the map, we need to save it.
To that please click `File` --> `Export Lanelet2Maps` then download.

After the download is finished,
we need to put lanelet2 map and pointcloud map on the same location.
The directory structure should be like this:

```diff
+ <YOUR-MAP-DIRECTORY>/
+  ├─ pointcloud_map.pcd
+  └─ lanelet2_map.osm
```

If your .osm or .pcd map file's name is different from these names,
you need to update autoware.launch.xml:

```diff
  <!-- Map -->
-  <arg name="lanelet2_map_file" default="lanelet2_map.osm" description="lanelet2 map file name"/>
+  <arg name="lanelet2_map_file" default="<YOUR-LANELET-MAP-NAME>.osm" description="lanelet2 map file name"/>
-  <arg name="pointcloud_map_file" default="pointcloud_map.pcd" description="pointcloud map file name"/>
+  <arg name="pointcloud_map_file" default="<YOUR-POINTCLOUD-MAP-NAME>.pcd" description="pointcloud map file name"/>
```

Now we are ready to launch the planning simulator:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=<YOUR-MAP-FOLDER-DIR> vehicle_model:=<YOUR-VEHICLE-MODEL> sensor_model:=<YOUR-SENSOR-KIT>
```

Example for tutorial_vehicle:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/Files/autoware_map/tutorial_map/ vehicle_model:=tutorial_vehicle sensor_model:=tutorial_vehicle_sensor_kit vehicle_id:=tutorial_vehicle
```

1. Click `2D Pose Estimate` button on rviz or press `P` and give a pose for initialization.
2. Click `2D Goal Pose` button on rviz or press `G` and give a pose for goal point.
3. You can see the stop line marker on the rviz screen.

Stop line markers on rviz:

<figure markdown>
  ![stop-line-test](images/stop-line-test.png){ align=center }
  <figcaption>
    Stop line test on the created map.
  </figcaption>
</figure>

You can check your stop line elements in the planning simulator as this demonstration video:

![type:video](https://youtube.com/embed/cAQ_ulo7LHo)
