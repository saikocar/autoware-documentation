# スピードバンプ

行動速度プランナーの[スピードバンプモジュール](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_crosswalk_module/)は、快適で安全な運転のために、
スピードバンプの前に速度が減速するように計画します。
それを操作するために、lanelet2 マップにスピードバンプを追加します。

## スピードバンプ要素の作成

ポイントクラウドマップにスピードバンプを作成するには、次の手順に従ってください:

1. Lanelet2Mapsセクションから`Linestring`を選択します。
2. クリックして速度バンプのポリゴンを描画します
3. 次に、Lanelet2Mapsセクションから`Linestring`を無効にしてください。
4. `Action`パネルから`Change to Polygon`をクリックします。
5. このポリゴンを選択し、タイプとして`speed_bump`を入力してください。
6. 次に、スピードバンプを追加するレーンレットをクリックしてください。
7. `Create General Regulatory ELement`を選択する。
8. この要素に移動し、`speed_bump`サブタイプとして入力してください。
9. `Add refers`をクリックして作成したスピードバンプポリゴンIDを入力します。

スピードバンプ作成のデモビデオで次の手順を確認できます:

![type:video](https://youtube.com/embed/EenccStyZVg)

### プランニングシミュレータを使用してスピードバンプ要素を作成したテスト

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

!!! 注記

    スピードバンプモジュールはデフォルトでは有効化されていません。有効化するには、[behavior_velocity_planner.param.yaml](https://github.com/autowarefoundation/autoware_launch/blob/main/autoware_launch/config/planning/scenario_planning/lane_driving/behavior_planning/behavior_velocity_planner/behavior_velocity_planner.param.yaml)をコメントから外してください。

これで、計画シミュレーターを起動する準備が整いました:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=<YOUR-MAP-FOLDER-DIR> vehicle_model:=<YOUR-VEHICLE-MODEL> sensor_model:=<YOUR-SENSOR-KIT>
```

チュートリアル車両の例:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/Files/autoware_map/tutorial_map/ vehicle_model:=tutorial_vehicle sensor_model:=tutorial_vehicle_sensor_kit vehicle_id:=tutorial_vehicle
```

1. rvizの`2D Pose Estimate`ボタンをクリックするか、`P`を押してポーズを与えると初期化されます。
2. rvizの`2D Goal Pose`ボタンをクリックするか、 `G`を押してゴールポイントのポーズを与えます。
3. rviz 画面にスピード バンプ マーカーが表示されます。

rviz のスピードバンプマーカー:

<figure markdown>
  ![speed-bump-test](images/speed-bump-test.png){ align=center }
  <figcaption>
    作成したマップ上でスピードバンプテストを行います。
  </figcaption>
</figure>

次のデモンストレーションビデオのように、計画シミュレーターでスピードバンプ要素を確認できます:

![type:video](https://youtube.com/embed/rg_a-ipdNAY)
