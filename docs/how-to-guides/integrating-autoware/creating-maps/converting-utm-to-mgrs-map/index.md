# UTMマップをMGRSマップ形式に変換する

## 概要

AutowareでMGRS(Military Grid Reference System)形式を使用する場合は、
UTM (ユニバーサル横メルカトル) マップを MGRS 形式に変換する必要があります。
これを行うには、Leo Driveが提供する[UTM to MGRS pointcloud converter](https://github.com/leo-drive/pc_utm_to_mgrs_converter)ROS 2パッケージを使用します。

## インストール

### 依存関係

- ROS 2
- PCL-conversions
- [GeographicLib](https://geographiclib.sourceforge.io/C++/doc/install.html)

依存関係をインストールするには:

```bash
sudo apt install ros-humble-pcl-conversions \
       geographiclib-tools
```

### ビルド

```bash
    cd <PATH-TO-YOUR-ROS-2-WORKSPACE>/src
    git clone https://github.com/leo-drive/pc_utm_to_mgrs_converter.git
    cd ..
    colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### 使用法

コンバータ ツールのインストール後、
ローカル UTM マップ原点の北距、
東距、および楕円体の高さを`pc_utm_to_mgrs_converter.param.yaml`で定義する必要があります。
たとえば、GNSS/INS センサーからの
navsatfixメッセージで緯度、経度、高度の値を使用できます。

??? note "Sample ROS 2 topic echo from navsatfix message"

    ```sh
    header:
    stamp:
    sec: 1694612439
    nanosec: 400000000
    frame_id: GNSS_INS/gnss_ins_link
    status:
    status: 0
    service: 1
    latitude: 41.0216110801253
    longitude: 28.887096461148346
    altitude: 74.28264078891529
    position_covariance:
    - 0.0014575386885553598
    - 0.0
    - 0.0
    - 0.0
    - 0.004014162812381983
    - 0.0
    - 0.0
    - 0.0
    - 0.0039727711118757725
    position_covariance_type: 2
    ```

その後、緯度と経度の値を北緯と東経の値に変換する必要があります。
緯度経度の値を UTM に変換するには、インターネット上の任意のコンバーターを使用できます。
(つまり、[UTMconverter](https://www.latlong.net/lat-long-utm.html))

これで、navsatfix メッセージの例を`pc_utm_to_mgrs_converter.param.yaml`に
更新する準備ができました:

```diff
/**:
  ros__parameters:
      # Northing of local origin
-     Northing: 4520550.0
+     Northing: 4542871.33

      # Easting of local origin
-     Easting: 698891.0
+     Easting: 658659.84

      # Elipsoid Height of local origin
-     ElipsoidHeight: 47.62
+     ElipsoidHeight: 74.28
```

最後に、`pc_utm_to_mgrs_converter.launch.xml`にある入力とポイントクラウドのマップパスを更新します:

```diff
...
- <arg name="input_file_path" default="/home/melike/projects/autoware_data/gebze_pospac_map/pointcloud_map.pcd"/>
+ <arg name="input_file_path" default="<PATH-TO-YOUR-INPUT-PCD-MAP>"/>
- <arg name="output_file_path" default="/home/melike/projects/autoware_data/gebze_pospac_map/pointcloud_map_mgrs_orto.pcd"/>
+ <arg name="output_file_path" default="<PATH-TO-YOUR-OUTPUT-PCD-MAP>"/>
...
```

パッケージの設定後、pc_utm_to_mgrs_converterを起動します:

```bash
ros2 launch pc_utm_to_mgrs_converter pc_utm_to_mgrs_converter.launch.xml
```

変換プロセスが開始され、
`Saved <YOUR-MAP-POINTS-SIZE> data points saved to <YOUR-OUTPUT-MAP-PATH>`メッセージが端末に表示されます。
MGRS形式の点群マップは、出力マップディレクトリに保存する必要があります。
