車両とセンサーのモデルの作成
概要
センサーモデル
目的:センサー モデルには、自動運転車で使用されるセンサーの校正 (変換) ファイルと起動ファイルが含まれています。これには、LiDAR、カメラ、レーダー、IMU (慣性測定ユニット)、GPS ユニットなどのさまざまなセンサーが含まれます。

重要:正確なセンサー モデリングは知覚タスクに不可欠です。正確なキャリブレーション値は、物体の検出、距離の推定、周囲の 3D 表現の作成などのセンサー データを処理することにより、環境を理解するのに役立ちます。

使用法:センサー モデルは、センサーの起動、パイプラインの構成、およびキャリブレーション値の記述のために Autoware で利用されます。

センサーモデル（センサーキット）は以下の3つのパッケージで構成されています。

common_sensor_launch
<YOUR_VEHICLE_NAME>_sensor_kit_description
<YOUR_VEHICLE_NAME>_sensor_kit_launch
個別のセンサー モデルの作成については、センサー モデルの作成ページを参照してください。

参考として、 Autoware のsample_sensor_kit_launchパッケージのフォルダー構造を次に示します。

sample_sensor_kit_launch/
├─ common_sensor_launch/
├─ sample_sensor_kit_description/
└─ sample_sensor_kit_launch/
車両型式
目的:車両モデルには、寸法を含む個々の車両仕様、車両の 3D モデル (.fbx または .dae 形式) などが含まれます。

重要:正確な車両モデルは、動作計画と制御にとって非常に重要です。

使用法:車両モデルは、車両の 3D モデルを含む Autoware の車両情報を提供するために Autoware で使用されます。

車両モデルは次の 2 つのパッケージで構成されます。

<YOUR_VEHICLE_NAME>_vehicle_description
<YOUR_VEHICLE_NAME>_vehicle_launch
個別の車両モデルの作成については、車両モデルの作成ページを参照してください。

参考として、Autoware のsample_vehicle_launchパッケージのフォルダー構造を次に示します。

sample_vehicle_launch/
├─ sample_vehicle_description/
└─ sample_vehicle_launch/
# Creating vehicle and sensor models

## Overview

### Sensor Model

- **Purpose:** The sensor model includes the calibration (transformation) and launch files of the
  sensors used in the autonomous vehicle.
  This includes various sensors like LiDARs, cameras,
  radars, IMUs (Inertial Measurement Units), GPS units, etc.

- **Importance:** Accurate sensor modeling is essential for perception tasks.
  Precise calibration values help understand the environment by processing sensor data,
  such as detecting objects, estimating distances,
  and creating a 3D representation of the surroundings.

- **Usage:** The sensor model is utilized in Autoware for launching sensors,
  configuring their pipeline, and describing calibration values.

- The sensor model (sensor kit) consists of the following three packages:
  - `common_sensor_launch`
  - `<YOUR_VEHICLE_NAME>_sensor_kit_description`
  - `<YOUR_VEHICLE_NAME>_sensor_kit_launch`

Please refer to the [creating sensor model](./creating-sensor-model) page
for creating your individual sensor model.

For reference,
here is the folder structure for the [sample_sensor_kit_launch](https://github.com/autowarefoundation/sample_sensor_kit_launch) package in Autoware:

```diff
sample_sensor_kit_launch/
├─ common_sensor_launch/
├─ sample_sensor_kit_description/
└─ sample_sensor_kit_launch/
```

### Vehicle Model

- **Purpose:** The vehicle model includes individual vehicle specifications with dimensions,
  a 3D model of the vehicle (in .fbx or .dae format), etc.

- **Importance:** An accurate vehicle model is crucial for motion planning and control.

- **Usage:** The vehicle model is employed in Autoware to provide vehicle information for Autoware,
  including the 3D model of the vehicle.

- The vehicle model comprises the following two packages:
  - `<YOUR_VEHICLE_NAME>_vehicle_description`
  - `<YOUR_VEHICLE_NAME>_vehicle_launch`

Please consult the [creating vehicle model](./creating-vehicle-model) page
for creating your individual vehicle model.

As a reference,
here is the folder structure for the [sample_vehicle_launch](https://github.com/autowarefoundation/sample_vehicle_launch) package in Autoware:

```diff
sample_vehicle_launch/
├─ sample_vehicle_description/
└─ sample_vehicle_launch/
```
