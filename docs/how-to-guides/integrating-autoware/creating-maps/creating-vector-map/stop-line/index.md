# 停止線

行動速度プランナーの[停止線モジュール](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_stop_line_module/)は、停止線の直前で停止し、
停止後に運転を再開する速度を計画します。
それを操作するために、lanlet2マップに停止線属性を追加します。

## 停止線規制要素の作成

ポイントクラウド マップ上に停止線を作成するには、次の手順に従ってください:

1. 追加する停止線を選択してください。
2. 上部パネルの`Abstraction`ボタンをクリックします。
3. パネルから`Stop Line`を選択します。
4. 停止線を挿入したい領域をクリックします。

停止線作成のデモンストレーション ビデオでこれらの手順を確認できます:

![type:video](https://youtube.com/embed/cgTSA50Yfyo)

### 計画シミュレータを使用して停止線要素を作成したテスト

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

.osmまたは.pcdマップファイルの名前がこれらの名前と異なる場合は、autoware.launch.xmlを更新する必要があります。
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
2. rvizの`2D Goal Pose`ボタンをクリックするか、 `G`を押してゴールポイントのポーズを与えます。 or press  and give a pose for goal point.
3. rviz画面上に停止線マーカーが表示されます。

rvizの停止線マーカー:

<figure markdown>
  ![stop-line-test](images/stop-line-test.png){ align=center }
  <figcaption>
    作成した地図上で停止線テストを行います。
  </figcaption>
</figure>

次のデモビデオのように、計画シミュレーターで停止線要素を確認できます:

![type:video](https://youtube.com/embed/cAQ_ulo7LHo)
