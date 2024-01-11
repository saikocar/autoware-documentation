# 信号機

行動速度プランナーの[信号機モジュール](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_traffic_light_module/)は、
信号機の状態に従って速度を計画します。
これを操作するために、lanelet2マップに信号属性を追加します。

## 信号機規制要素の作成

ポイントクラウド マップ上に信号機を作成するには、次の手順に従ってください:

1. どの信号機を追加するレーンレットを選択してください。
2. 上部パネルの`Abstraction`ボタンをクリックします。
3. パネルから`Traffic Light`を選択します。
4. 信号機を挿入する目的の領域をクリックします。

これらの手順は、信号機作成のデモンストレーション ビデオで見ることができます:

![type:video](https://youtube.com/embed/P3xcayPkTOg)

### 計画シミュレータを使用して信号機要素を作成したテスト

マップの作成が完了したら、マップを保存する必要があります。
`File` --> `Export Lanelet2Maps`をクリックしてダウンロードしてください。

ダウンロードが完了したら、
lanelet2マップとpointcloudマップを同じ場所に配置する必要があります。
ディレクトリ構造は次のようになります:

```diff
+ <YOUR-MAP-DIRECTORY>/
+  ├─ pointcloud_map.pcd
+  └─ lanelet2_map.osm
```

.osmまたは.pcdマップファイルの名前がこれらの名前と異なる場合は、
autoware.launch.xml を更新する必要があります:

```diff
  <!-- Map -->
-  <arg name="lanelet2_map_file" default="lanelet2_map.osm" description="lanelet2 map file name"/>
+  <arg name="lanelet2_map_file" default="<YOUR-LANELET-MAP-NAME>.osm" description="lanelet2 map file name"/>
-  <arg name="pointcloud_map_file" default="pointcloud_map.pcd" description="pointcloud map file name"/>
+  <arg name="pointcloud_map_file" default="<YOUR-POINTCLOUD-MAP-NAME>.pcd" description="pointcloud map file name"/>
```

これで、計画シミュレーターを起動する準備が整いました:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=<YOUR-MAP-FOLDER-DIR> vehicle_model:=<YOUR-VEHICLE-MODEL> sensor_model:=<YOUR-SENSOR-KIT>
```

チュートリアル車両の例:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/Files/autoware_map/tutorial_map/ vehicle_model:=tutorial_vehicle sensor_model:=tutorial_vehicle_sensor_kit vehicle_id:=tutorial_vehicle
```

1. rvizの`2D Pose Estimate`ボタンをクリックするか、`P`を押してポーズを与えると初期化されます。
2. `Panels` -> `Add new panel`をクリックし、`TrafficLightPublishPanel`を選択し、`OK`を押します。
3. TrafficLightPublishPanelで、信号機のIDと色を設定します。
4. `SET`と`PUBLISH`ボタンをクリックします。
5. rvizの`2D Goal Pose`ボタンをクリックするか、`G`を押してゴールポイントのポーズを与えます。
6. 信号機の色を`RED`に設定すると、rviz 画面に信号機マーカーが表示されます。

rvizの信号マーカー:

<figure markdown>
  ![traffic-light-test](images/traffic-light-test.png){ align=center }
  <figcaption>
    作成した地図上の信号機テスト。
  </figcaption>
</figure>

次のデモビデオのように、計画シミュレーターで信号機の要素を確認できます:

![type:video](https://youtube.com/embed/AaFT24uqbJk)
