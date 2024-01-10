# 横断歩道属性

行動速度プランナーの[横断歩道モジュール](https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_velocity_crosswalk_module/)は、
横断歩道に近づいたり歩いたりする歩行者に対して停止または減速する速度を計画します。
これを操作するために、lanelet2マップに横断歩道属性を追加します。

## 横断歩道属性の作成

地図上に横断歩道を作成するには、次の手順に従ってください:

1. トップパネルの`Abstraction`ボタンをクリックします。
2. `Crosswalk`パネルから選択します。
3. ポイントクラウドマップ上で横断歩道をクリックして描画します。

横断歩道作成のデモンストレーションビデオで次の手順を確認できます:

![type:video](https://youtube.com/embed/J6WrL8dkFhI)

### 計画シミュレータで作成した横断歩道のテスト

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
autoware.launch.xmlを更新する必要があります:

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

1. `2D Pose Estimate`のボタンをクリックするか、`P`を押してポーズを与えると初期化されます。
2. `2D Goal Pose`のボタンをクリックするか、`G`を押してゴールポイントのポーズを与えます。
3. 横断歩道に歩行者を追加する必要があるため、`Tool Properties`のパネルからインタラクティブな歩行者をアクティブにします。
4. その後、`Shift`を押し、右クリックボタンをクリックして歩行者を挿入してください。
5. 挿入された歩行者を右クリックでドラッグして制御できます。

rvizの横断歩道マーカー:

<figure markdown>
  ![crosswalk-test](images/crosswalk-test.png){ align=center }
  <figcaption>
    作成した地図上で横断歩道テストを行います。
  </figcaption>
</figure>

次のデモビデオのように、計画シミュレーターで横断歩道要素を確認できます:

![type:video](https://youtube.com/embed/hhwBku_1qmA)
