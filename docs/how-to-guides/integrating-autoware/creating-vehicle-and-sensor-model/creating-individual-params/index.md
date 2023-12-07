個別のパラメータの作成
導入
Individual_paramsパッケージは、さまざまな車両向けにカスタマイズされたセンサーのキャリブレーションを定義するために使用されます。同じセンサー モデルで同じ起動ファイルを使用しながら、さまざまな車両に合わせてカスタマイズされたセンサー キャリブレーションを定義できます。

!!! 警告

The "individual_params" package contains the calibration
results for your sensor kit and overrides the default calibration results found in
VEHICLE-ID_sensor_kit_description/config/ directory.
individual_parametersリポジトリを Autoware 内に配置する
このガイドの前に、リポジトリをフォークして、ガイドのこのセクションの例として使用するtutorial_vehicle_individual_paramsautoware_individual_paramsリポジトリを作成しました。Individual_parameters リポジトリは、以下に示すものと同じフォルダー構造に従って、Autoware フォルダー内に配置する必要があります。

??? 注「サンプルフォルダー構造tutorial_vehicle_individual_params」

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
その後、individual_paramsパッケージをビルドする必要があります。

colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-up-to individual_params
これで、 vehicle_id を引数として Autoware を使用する準備が整いました。たとえば、異なるセンサー キャリブレーション要件を持つ複数の同様の車両の場合、autoware_individual_params 構造は次のようになります。

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
次に、次のように vehicle_id 引数を指定して autoware を使用できます。

を引数として追加し<vehicle_id>、起動時にオプションでパラメータを切り替えます。

# example1 (do not set vehicle_id)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle
# example2 (set vehicle_id as VEHICLE_1)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle vehicle_id:=VEHICLE_1
# example3 (set vehicle_id as VEHICLE_2)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle vehicle_id:=VEHICLE_2

# Creating individual params

## Introduction

The [individual_params](https://github.com/autowarefoundation/autoware_individual_params) package is used
to define customized sensor calibrations for different vehicles.
It lets
you define customized sensor calibrations for different vehicles
while using the same launch files with the same sensor model.

!!! Warning

    The "individual_params" package contains the calibration
    results for your sensor kit and overrides the default calibration results found in
    VEHICLE-ID_sensor_kit_description/config/ directory.

## Placing your `individual_parameters` repository inside Autoware

[Previously on this guide](../../creating-your-autoware-repositories/creating-autoware-repositories.md),
we forked the `autoware_individual_params` repository
to create a [tutorial_vehicle_individual_params](https://github.com/leo-drive/tutorial_vehicle_individual_params) repository
which will be used as an example for this section of the guide.
Your individual_parameters repository should be placed inside your Autoware folder following the same folder structure as the one shown below:

??? note "sample folder structure for [`tutorial_vehicle_individual_params`](https://github.com/leo-drive/tutorial_vehicle_individual_params)"

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

After that, we need to build our `individual_params` package:

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-up-to individual_params
```

Now you are ready to use Autoware with a vehicle_id as an argument.
For example, if you are several, similar vehicles with different sensor calibration requirements,
your autoware_individual_params structure should look like this:

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

Then, you can use autoware with vehicle_id arguments like this:

Add a `<vehicle_id>` as an argument and switch parameters using options at startup.

```bash
# example1 (do not set vehicle_id)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle
# example2 (set vehicle_id as VEHICLE_1)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle vehicle_id:=VEHICLE_1
# example3 (set vehicle_id as VEHICLE_2)
$ ros2 launch autoware_launch autoware.launch.xml sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit vehicle_model:=<YOUR-VEHICLE-NAME>_vehicle vehicle_id:=VEHICLE_2
```
