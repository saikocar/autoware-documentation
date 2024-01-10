# 検出エリア要素

行動速度プランナの[検知エリア](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_detection_area_module/)は、地図上に定義された検知エリア内に点群が検知された場合に、
所定の地点で停止計画を実行する際の速度を計画します。
これを操作するために、lanelet2マップに検出エリア要素を追加します。

## 検出エリア要素の作成

マップ上に検出エリアを作成するには、次の手順に従ってください:

1. トップパネルの`Lanelet2Maps`をクリックします。
2. パネルから`Detection Area`を選択します。
3. 追加する停止線を選択してください。
4. 点群マップをクリックして`Detection Area`を挿入します。
5. 検出領域の隅にある点をクリックすると、検出領域の寸法を変更できます。詳細については、デモビデオをご覧ください。

検出エリア作成のデモビデオでこれらの手順を確認できます:

![type:video](https://youtube.com/embed/RUJvXok-ncQ)

### 作成した検知エリアをプランニングシミュレーターでテスト

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

.osm または .pcd マップ ファイルの名前がこれらの名前と異なる場合は、
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
2. rvizの`2D Goal Pose`ボタンをクリックするか、 `G`を押してゴールポイントのポーズを与えます。
3. 歩行者を検出エリアに追加する必要があるため、rvizの`Tool Properties`パネルからインタラクティブな歩行者を有効にします。
4. その後、`Shift`を押し、右クリックボタンをクリックして歩行者を挿入してください。
5. 挿入された歩行者を右クリックでドラッグして制御できます。したがって、テストのために歩行者を検出エリアに置く必要があります。

rviz上の停止検出領域:

<figure markdown>
  ![detection-area-test](images/detection-area-test.png){ align=center }
  <figcaption>
    作成したマップ上の検出エリアテスト。
  </figcaption>
</figure>

次のデモビデオのように、計画シミュレーターで検出エリアの要素を確認できます:

![type:video](https://youtube.com/embed/zjfPnRIz8Xk)
