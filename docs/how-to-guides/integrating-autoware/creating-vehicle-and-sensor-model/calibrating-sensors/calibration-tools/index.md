# TIER IV のキャリブレーションツールで始める

## 概要

Autoware は、知覚、位置特定、
計画スタックへの入力として複数のセンサーが
車両に取り付けられることを期待しています。
これらのセンサーは正しく校正する必要があり、
その位置は`sensor_kit_description`と`individual_params`パッケージで定義する必要があります。
このチュートリアルでは、
キャリブレーションにTIER IV の[CalibrationTools](https://github.com/tier4/CalibrationTools)CalibrationToolsリポジトリを使用します。

## base_linkに対するsensor_kit_base_linkの位置の設定

前のセクション (車両とセンサー モデルの作成) で、
`sensors_calibration.yaml`について説明しました。
このファイルには、`base_link`（親フレーム）に対する`sensor_kit_base_link`（子フレーム）の位置と向きが保存されます。
`base_link`（親フレーム）.
車両の CAD データを使用して、この相対位置
(ファイルの作成時にすべての値が最初にゼロに設定されています)
を更新する必要があります。

<figure markdown>
  ![ackermann_link](images/tutorial_vehicle_sensor_kit_base_link.png){ align=center }
  <figcaption>
  tutorial_vehicleのbase_linkからsensor_kit_base_linkへの変換。
  </figcaption>
</figure>

したがって、tutorial_vehicleの`sensors_calibration.yaml`ファイルは次のようになります。:

```yaml
base_link:
  sensor_kit_base_link:
    x: 1.600000 # meter
    y: 0.0
    z: 1.421595 # 1.151595m + 0.270m
    roll: 0.0
    pitch: 0.0
    yaw: 0.0
```

`sensor_kit_base_link`フレームに関してこの変換値を更新する必要があります。
`sensor_kit_calibration.yaml`ファイル内の GNSS/INS および IMU 位置の CAD 値を使用することもできます。
(sensor_kit_launch パッケージと Individual_params パッケージの両方で
sensor_kit_calibration.yaml ファイルを更新することを忘れないでください)

## TIER IVのCalibrationToolsリポジトリをautoware にインストールする

前の手順 (独自のオートウェアの作成、車両と
センサーモデルの作成など) を完了したら、
センサーモデルセクションの作成でパイプラインを準備したセンサーを調整する準備が整いました。

まず、独自のオートウェア内に CalibrationTools リポジトリのクローンを作成します。

```bash
cd <YOUR-OWN-AUTOWARE-DIRECTORY> # for example: cd autoware.tutorial_vehicle
wget https://raw.githubusercontent.com/tier4/CalibrationTools/tier4/universe/calibration_tools.repos
vcs import src < calibration_tools.repos
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

次に、センサー モデルと車両モデルに必要な変更をすべて加えた後、
すべてのパッケージをビルドします。

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## キャリブレーションツールの使用法

CalibrationTools リポジトリには、
ライダーとライダー、カメラとライダー、地上とライダーなどの
さまざまなセンサー ペアを校正するためのパッケージがいくつかあり、
センサーを校正するには、`extrinsic_calibration_package` センサーキットを自分用に変更を加えます。

tutorial_vehicleの場合、
チュートリアル セクションに続いて作成された完成した起動ファイルは、[ここ](https://github.com/leo-drive/calibration_tools_tutorial_vehicle/tree/tutorial_vehicle/sensor/extrinsic_calibration_manager/launch/tutorial_vehicle_sensor_kit)にあります。

- [手動校正](../extrinsic-manual-calibration)
- [ライダー間キャリブレーション](../lidar-lidar-calibration)
  - [地表とライダーのキャリブレーション](../ground-lidar-calibration)
- [固有のカメラキャリブレーション](../intrinsic-camera-calibration)
- [LiDAR-カメラのキャリブレーション](../lidar-camera-calibration)
