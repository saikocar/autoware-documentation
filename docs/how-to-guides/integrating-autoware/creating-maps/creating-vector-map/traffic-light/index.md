信号機
行動速度プランナーの信号機モジュールは、信号機の状態に従って速度を計画します。これを操作するために、lanelet2 マップに信号属性を追加します。

信号機規制要素の作成
ポイントクラウド マップ上に信号機を作成するには、次の手順に従ってください。

どの信号機を追加するレーンレットを選択してください。
トップパネルの ボタンをクリックしますAbstraction。
Traffic Lightパネルから選択します。
信号機を挿入する目的の領域をクリックします。
これらの手順は、信号機作成のデモンストレーション ビデオで見ることができます。

タイプ:ビデオ

計画シミュレータを使用して信号機要素を作成したテスト
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
Panels->をクリックしAdd new panel、 を選択しTrafficLightPublishPanel、 を押しますOK。
TrafficLightPublishPanel で、信号機の ID と色を設定します。
次に、SETとPUBLISHボタンをクリックします。
2D Goal Poserviz のボタンをクリックするか、 を押してGゴールポイントのポーズをとります。
信号機の色を に設定すると、rviz 画面に信号機マーカーが表示されますRED。
rviz の信号マーカー:

![traffic-light-test](images/traffic-light-test.png){ align=center } 作成した地図上の信号機テスト。
次のデモビデオのように、計画シミュレーターで信号機の要素を確認できます。

タイプ:ビデオ
# Traffic light

Behavior velocity planner's [traffic light module](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_traffic_light_module/) plans velocity
according to the traffic light status.
In order to operate that, we will add traffic light attribute to our lanelet2 map.

## Creating a traffic light regulatory element

In order to create a traffic light on your pointcloud map, please follow these steps:

1. Please select lanelet which traffic light to be added.
2. Click `Abstraction` button on top panel.
3. Select `Traffic Light` from the panel.
4. Click on the desired area for inserting traffic light.

You can see these steps in the traffic-light creating demonstration video:

![type:video](https://youtube.com/embed/P3xcayPkTOg)

### Testing created the traffic light element with planning simulator

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
2. Click `Panels` -> `Add new panel`, select `TrafficLightPublishPanel`, and then press `OK`.
3. In TrafficLightPublishPanel, set the ID and color of the traffic light.
4. Then, Click `SET` and `PUBLISH` button.
5. Click `2D Goal Pose` button on rviz or press `G` and give a pose for goal point.
6. You can see the traffic light marker on the rviz screen if you set the traffic light color as `RED`.

Traffic Light markers on rviz:

<figure markdown>
  ![traffic-light-test](images/traffic-light-test.png){ align=center }
  <figcaption>
    Traffic light test on the created map.
  </figcaption>
</figure>

You can check your traffic light elements in the planning simulator as this demonstration video:

![type:video](https://youtube.com/embed/AaFT24uqbJk)
