# 個別のパラメータの作成

## 導入

[individual_params](https://github.com/autowarefoundation/autoware_individual_params)パッケージは、さまざまな車両向けにカスタマイズされたセンサーの
キャリブレーションを定義するために使用されます
同じセンサー モデルで同じ起動ファイルを使用しながら、
さまざまな車両に合わせてカスタマイズされた
センサー キャリブレーションを定義できます。

!!! 警告

    "individual_params"パッケージにはセンサー キットのキャリブレーション結果が含まれており、
    VEHICLE-ID_sensor_kit_description/config/ ディレクトリにある
    デフォルトのキャリブレーション結果をオーバーライドします。

## `individual_parameters`individual_parametersリポジトリをAutoware内に配置する

[このガイドの前に](../../creating-your-autoware-repositories/creating-autoware-repositories.md)、
`autoware_individual_params`リポジトリをフォークして、
ガイドのこのセクションの例として使用する
[tutorial_vehicle_individual_params](https://github.com/leo-drive/tutorial_vehicle_individual_params)リポジトリを作成しました。
individual_parametersリポジトリは、以下に示すものと同じフォルダー構造に従って、Autowareフォルダー内に配置する必要があります:

??? 注記 "[`tutorial_vehicle_individual_params`](https://github.com/leo-drive/tutorial_vehicle_individual_params)のサンプルフォルダー構造"

    ```diff
      <YOUR-OWN-AUTOWARE-DIR>/
      └─ src/
      └─ param/
      └─ tutorial_vehicle_individual_params/
      └─ individual_params/
      └─ config/
      ├─ default/
    + └─ tutorial_vehicle/
    +     └─ tutorial_vehicle_sensor_kit_launch/
    +         ├─ imu_corrector.param.yaml
    +         ├─ sensor_kit_calibration.yaml
    +         └─ sensors_calibration.yaml
    ```

その後、`individual_params`パッケージをビルドする必要があります:

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-up-to individual_params
```

これで、 vehicle_id を引数として Autoware を使用する準備が整いました。
たとえば、異なるセンサー キャリブレーション要件を持つ複数の同様の車両の場合、
autoware_individual_params 構造は次のようになります:

```diff
individual_params/
└─ config/
     ├─ default/
     │   └─ <YOUR_SENSOR_KIT>/                  # example1
     │        ├─ imu_corrector.param.yaml
     │        ├─ sensor_kit_calibration.yaml
     │        └─ sensors_calibration.yaml
+    ├─ VEHICLE_1/
+    │   └─ <YOUR_SENSOR_KIT>/                  # example2
+    │        ├─ imu_corrector.param.yaml
+    │        ├─ sensor_kit_calibration.yaml
+    │        └─ sensors_calibration.yaml
+    └─ VEHICLE_2/
+         └─ <YOUR_SENSOR_KIT>/                  # example3
+              ├─ imu_corrector.param.yaml
+              ├─ sensor_kit_calibration.yaml
+              └─ sensors_calibration.yaml
```

次に、以下のようにvehicle_id引数を指定してautowareを使用できます:

`<vehicle_id>` を引数として追加し、起動時にオプションでパラメータを切り替えます。

```bash
# example1 (do not set vehicle_id)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle
# example2 (set vehicle_id as VEHICLE_1)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle vehicle_id:=VEHICLE_1
# example3 (set vehicle_id as VEHICLE_2)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle vehicle_id:=VEHICLE_2
```
