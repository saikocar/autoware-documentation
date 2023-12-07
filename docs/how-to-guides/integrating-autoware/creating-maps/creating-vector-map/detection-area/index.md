検出エリア要素
行動速度プランナの検知エリアは、地図上に定義された検知エリア内に点群が検知された場合に、所定の地点で停止計画を実行する際の速度を計画します。これを操作するために、lanelet2 マップに検出エリア要素を追加します。

検出エリア要素の作成
マップ上に検出エリアを作成するには、次の手順に従ってください。

トップパネルの ボタンをクリックしますLanelet2Maps。
Detection Areaパネルから選択します。
追加する停止線を選択してください。
Detection Area点群マップをクリックして挿入します。
検出領域の隅にある点をクリックすると、検出領域の寸法を変更できます。詳細については、デモビデオをご覧ください。
検出エリア作成のデモビデオでこれらの手順を確認できます。

タイプ:ビデオ

作成した検知エリアをプランニングシミュレーターでテスト
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
歩行者を検出エリアに追加する必要があるため、Tool Propertiesrviz のパネルからインタラクティブな歩行者を有効にします。
その後、 を押しShift、右クリックボタンをクリックして歩行者を挿入してください。
挿入された歩行者を右クリックでドラッグして制御できます。したがって、テストのために歩行者を検出エリアに置く必要があります。
rviz 上の停止検出領域:

![検出エリアテスト](images/検出エリアテスト.png){ align=center } 作成したマップ上の検出エリアテスト。
次のデモ ビデオのように、計画シミュレーターで検出エリアの要素を確認できます。

タイプ:ビデオ
# Detection area element

Behavior velocity planner's [detection area](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_detection_area_module/) plans velocity
when if pointcloud is detected in a detection area defined on a map, the stop planning will be executed at the predetermined point.
In order to operate that, we will add a detection area element to our lanelet2 map.

## Creating a detection area element

In order to create a detection area on your map, please follow these steps:

1. Click `Lanelet2Maps` button on top panel.
2. Select `Detection Area` from the panel.
3. Please select lanelet which stop line to be added.
4. Click and insert `Detection Area` on your pointcloud map.
5. You can change the dimensions of the detection area with clicking points on the corners of the detection area. For more information, you can check the demonstration video.

You can see these steps in the detection area creating demonstration video:

![type:video](https://youtube.com/embed/RUJvXok-ncQ)

### Testing created detection area with planning simulator

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
3. We need to add pedestrians to detection area, so activate interactive pedestrians from `Tool Properties` panel on rviz.
4. After that, please press `Shift`, then click right click button for inserting pedestrians.
5. You can control inserted pedestrian via dragging right click. So, you should put pedestrian on the detection area for testing.

Stop detection area on rviz:

<figure markdown>
  ![detection-area-test](images/detection-area-test.png){ align=center }
  <figcaption>
    Detection area test on the created map.
  </figcaption>
</figure>

You can check your detection area elements in the planning simulator as this demonstration video:

![type:video](https://youtube.com/embed/zjfPnRIz8Xk)
