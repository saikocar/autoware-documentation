# レーンレットの作成

このページでは、点群マップ上に簡単なレーンレットを作成する方法を説明します。
これまで点群マップを持っていなかった場合は、
[LIO-SAMマッピング ページ](../../open-source-slam/lio-sam)の
Autoware用の点群マップの作成方法を
確認して手順に従ってください。

## レーンレット2の作成

まず、点群マップを Vector Map Builder toolにインポートする必要があります:

1. `File`をクリックしてください。
2. 次に、`Import PCD` をクリックします。
3. `Browse`をクリックして.pcdファイルを選択します。

アップロードが完了したら、Vector Map Builderツールに点群を表示します:

<figure markdown>
  ![pointcloud-map](images/pointcloud-map.png){ align=center }
  <figcaption>
    Vector Map Builderにアップロードされたpointcloudマップファイル
  </figcaption>
</figure>

これで、点群マップ上にlanelet2マップを作成する準備が整いました:

1. `Create`クリックをしてください。
2. 次に、`Create Lanelet2Maps`をクリックします。
3. マップ名を入力してください。
4. MGRSゾーンを入力してください。 (チュートリアル車両では, MGRS grid zone: 35T - MGRS 100,000-meter square: PF)
5. Click `Create`をクリックしてください。

### 簡単なレーンレットの作成

マップ上に単純なレーンレットを作成するには、次の手順に従ってください:

1. バーの`Lanelet2Maps`をクリックします。
2. `Lanelet`を選択してLaneletモードを有効にします。
3. 次に、点群マップをクリックしてレーンレットを作成できます。
4. レーンレットが終了した場合は、`Lanelet`を無効にすることができます。
5. レーンレットの幅を変更したい場合は、`lanelet` --> `Change Lanelet Width`をクリックして、レーンレットの幅を入力できます。

ビデオデモンストレーション:

![type:video](https://youtube.com/embed/183PHi84AeU)

### 2つのレーンレットを結合する

2つのレーンレットを結合するには、次の手順に従ってください:

1. 2つの異なるレーンレットを作成してください。
2. レーンレットを選択し、`Shift`を押して他のレーンレットを選択します。
3. `Join Lanelets`ボタンが表示されるので、それを押してください。
4. これらのレーンレットは結合されます。

ビデオデモンストレーション:

![type:video](https://youtube.com/embed/_tHilFUKDQc)

### 複数のレーンレットを結合する

2つ以上のレーンレットを別のレーンレットに追加(結合)するには、次の手順に従ってください:

1. 複数のレーンレットを作成します。
2. 前の手順と同様に、最初の2つのレーンレットを結合できます。
3. 最初のレーンレットのエンドポイントIDを確認してください.
4. 次に、これらのIDを3番目のレーンレットの開始点に変更する必要があります。(レーンレットの折れ線選択で変更してください)
5. 最初のレーンレットの次の2つのレーンが表示されることがわかります。

ビデオデモンストレーション:

![type:video](https://youtube.com/embed/l5ZnL0Cjmnk)

### レーンレットの制限速度を変更する

レーンレットの制限速度を変更するには、次の手順に従ってください:

1. 制限速度を変更するレーンレットを選択してください。
2. 右の`speed limit`パネルで設定します。

### 計画シミュレーターを使用してレーンレットをテストする

レーンレットの作成が完了したら、保存する必要があります。
File` --> `Export Lanelet2Maps` をクリックしてダウンロードしてください。

ダウンロードが完了したら、
lanelet2 マップと pointcloud マップを同じ場所に配置する必要があります。
ディレクトリ構造は次のようになります:

```diff
<YOUR-MAP-DIRECTORY>/
 ├─ pointcloud_map.pcd
 └─ lanelet2_map.osm
```

.osmまたは.pcdマップファイルの名前がこれらの名前と異なる場合は、
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

チュートリアル車輛の例:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/Files/autoware_map/tutorial_map/ vehicle_model:=tutorial_vehicle sensor_model:=tutorial_vehicle_sensor_kit vehicle_id:=tutorial_vehicle
```

1. rvizの`2D Pose Estimate`ボタンをクリックするか、`P`を押してポーズを与えると初期化されます。
2. `2D Goal Pose`のボタンをクリックするか、`G`を押してゴールポイントのポーズをとります。

<figure markdown>
  ![planning-simulator-test](images/planning-simulator-map-test.png){ align=center }
  <figcaption>
    作成したベクターップを計画シミュレーターでテストする
  </figcaption>
</figure>
