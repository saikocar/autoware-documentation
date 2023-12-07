スピードバンプ
行動速度プランナーのスピード バンプ モジュールは、快適で安全な運転のために、スピード バンプの前に速度が減速するように計画します。それを操作するために、lanelet2 マップにスピード バンプを追加します。

スピードバンプ要素の作成
ポイントクラウド マップにスピード バンプを作成するには、次の手順に従ってください。

LinestringLanelet2Maps セクションから選択します。
クリックして速度バンプの多角形を描画します。
Linestring次に、 Lanelet2Maps セクションから無効にしてください。
Change to PolygonパネルからクリックしますAction。
speed_bumpこのポリゴンを選択し、タイプとして入力してください。
次に、どのスピードバンプを追加するレーンレットをクリックしてください。
選択するCreate General Regulatory ELement。
この要素に移動し、speed_bumpサブタイプとして入力してください。
クリックしてAdd refers、作成したスピード バンプ ポリゴン ID を入力します。
スピード バンプ作成のデモ ビデオで次の手順を確認できます。

タイプ:ビデオ

プランニングシミュレータを使用してスピードバンプ要素を作成したテスト
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
!!! 注記

The speed bump module not enabled default. To enable that, please uncomment it your [behavior_velocity_planner.param.yaml](https://github.com/autowarefoundation/autoware_launch/blob/main/autoware_launch/config/planning/scenario_planning/lane_driving/behavior_planning/behavior_velocity_planner/behavior_velocity_planner.param.yaml).
これで、計画シミュレーターを起動する準備が整いました。

ros2 launch autoware_launch planning_simulator.launch.xml map_path:=<YOUR-MAP-FOLDER-DIR> vehicle_model:=<YOUR-VEHICLE-MODEL> sensor_model:=<YOUR-SENSOR-KIT>
チュートリアル_車両の例:

ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/Files/autoware_map/tutorial_map/ vehicle_model:=tutorial_vehicle sensor_model:=tutorial_vehicle_sensor_kit vehicle_id:=tutorial_vehicle
2D Pose Estimaterviz の ボタンをクリックするか、 を押してPポーズを与えると初期化されます。
2D Goal Poserviz のボタンをクリックするか、 を押してGゴールポイントのポーズをとります。
rviz 画面にスピード バンプ マーカーが表示されます。
rviz のスピード バンプ マーカー:

![speed-bump-test](images/speed-bump-test.png){ align=center } 作成したマップ上でスピードバンプテストを行います。
次のデモンストレーション ビデオのように、計画シミュレーターでスピード バンプ要素を確認できます。

タイプ:ビデオ
# Speed bump

Behavior velocity planner's [speed bump module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_crosswalk_module/) plans velocity
to slow down before speed bump for comfortable and safety driving.
In order to operate that, we will add speed bumps to our lanelet2 map.

## Creating a speed bump element

In order to create a speed bump on your pointcloud map, please follow these steps:

1. Select `Linestring` from Lanelet2Maps section.
2. Click and draw polygon for speed bump.
3. Then please disable `Linestring` from Lanelet2Maps section.
4. CLick `Change to Polygon` from the `Action` panel.
5. Please select this Polygon and enter `speed_bump` as the type.
6. Then, please click lanelet which speed bump to be added.
7. Select `Create General Regulatory ELement`.
8. Go to this element, and please enter `speed_bump` as subtype.
9. Click `Add refers` and type your created speed bump polygon ID.

You can see these steps in the speed bump creating demonstration video:

![type:video](https://youtube.com/embed/EenccStyZVg)

### Testing created the speed bump element with planning simulator

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

!!! note

    The speed bump module not enabled default. To enable that, please uncomment it your [behavior_velocity_planner.param.yaml](https://github.com/autowarefoundation/autoware_launch/blob/main/autoware_launch/config/planning/scenario_planning/lane_driving/behavior_planning/behavior_velocity_planner/behavior_velocity_planner.param.yaml).

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
3. You can see the speed bump marker on the rviz screen.

Speed bump markers on rviz:

<figure markdown>
  ![speed-bump-test](images/speed-bump-test.png){ align=center }
  <figcaption>
    Speed bump test on the created map.
  </figcaption>
</figure>

You can check your speed bump elements in the planning simulator as this demonstration video:

![type:video](https://youtube.com/embed/rg_a-ipdNAY)
