# 車両とセンサーのモデルの作成

## 概要

### センサーモデル

- **目的:** センサー モデルには、自動運転車で使用されるセンサーの校正 (変換) ファイルと
  起動ファイルが含まれています。
  これには、LiDAR、カメラ、レーダー、IMU (慣性測定ユニット)、
  GPS ユニットなどのさまざまなセンサーが含まれます。

- **重要:** 正確なセンサー モデリングは知覚タスクに不可欠です。
  正確なキャリブレーション値は、物体の検出、距離の推定、周囲の 3D 表現の作成などの
  センサー データを処理することにより、
  環境を理解するのに役立ちます。

- **使用法:** センサー モデルは、センサーの起動、パイプラインの構成、
  およびキャリブレーション値の記述のために Autoware で利用されます。

- センサーモデル（センサーキット）は以下の3つのパッケージで構成されています:
  - `common_sensor_launch`
  - `<YOUR_VEHICLE_NAME>_sensor_kit_description`
  - `<YOUR_VEHICLE_NAME>_sensor_kit_launch`

個別のセンサー モデルの作成については、[センサー モデルの作成](./creating-sensor-model)ページを
参照してください。

参考として、
Autowareの[sample_sensor_kit_launch](https://github.com/autowarefoundation/sample_sensor_kit_launch)パッケージのフォルダー構造を次に示します:

```diff
sample_sensor_kit_launch/
├─ common_sensor_launch/
├─ sample_sensor_kit_description/
└─ sample_sensor_kit_launch/
```

### 車両モデル

- **目的** 車両モデルには、寸法を含む個々の車両仕様、
  車両の 3D モデル (.fbx または .dae 形式) などが含まれます。

- **重要:** 正確な車両モデルは、動作計画と制御にとって非常に重要です。

- **使用法:** T車両モデルは、車両の 3D モデルを含む Autoware の
  車両情報を提供するために Autoware で使用されます。

- 車両モデルは次の 2 つのパッケージで構成されます:
  - `<YOUR_VEHICLE_NAME>_vehicle_description`
  - `<YOUR_VEHICLE_NAME>_vehicle_launch`

個別の車両モデルの作成については、[車両モデルの作成](./creating-vehicle-model)
ページを参照してください。

参考として、
Autoware の[sample_vehicle_launch](https://github.com/autowarefoundation/sample_vehicle_launch)パッケージのフォルダー構造を次に示します。:

```diff
sample_vehicle_launch/
├─ sample_vehicle_description/
└─ sample_vehicle_launch/
```
