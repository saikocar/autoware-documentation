# 概要

## 要件: 実車のハードウェアを準備する

車両の前提条件:

- [Autowareインストールの前提条件](../../installation/autoware/source-installation.md#prerequisites)を満たす車載コンピューター
- 以下のデバイスが接続されています
  - ドライブバイワイヤインターフェース
  - LiDAR
  - オプション: Inertial measurement unit
  - オプション: Camera
  - オプション: GNSS

## 1. Autowareメタリポジトリの作成

Autowareメタリポジトリを作成します。
簡単な方法の1つは、[autowarefoundation/autoware](https://github.com/autowarefoundation/autoware)をフォークしてクローンを作成することです。
リポジトリをフォークする方法については[GitHub Docs](https://docs.github.com/en/get-started/quickstart/fork-a-repo)を参照してください。

```bash
git clone https://github.com/YOUR_NAME/autoware.git
```

複数の種類の車両を設定する場合は"autoware.vehicle_A"や"autoware.vehicle_B"のようなサフィックスを追加することをお勧めします

## 2. 車両とセンサーの説明を作成する
2. 車両とセンサーの説明を作成する
次に、車両と車両のセンサー構成を定義する記述パッケージを作成する必要があります。

次の 2 つのパッケージを作成します。

YOUR_VEHICLE_launch (例はここを参照)
YOUR_SENSOR_KIT_launch (例はここを参照)
autoware.repos作成したら、クローンされた Autoware リポジトリのファイルを更新して、これら 2 つの説明パッケージを参照する必要があります。

-  # sensor_kit
-  sensor_kit/sample_sensor_kit_launch:
-    type: git
-    url: https://github.com/autowarefoundation/sample_sensor_kit_launch.git
-    version: main
-  # vehicle
-  vehicle/sample_vehicle_launch:
-    type: git
-    url: https://github.com/autowarefoundation/sample_vehicle_launch.git
-    version: main
+  # sensor_kit
+  sensor_kit/YOUR_SENSOR_KIT_launch:
+    type: git
+    url: https://github.com/YOUR_NAME/YOUR_SENSOR_KIT_launch.git
+    version: main
+  # vehicle
+  vehicle/YOUR_VEHICLE_launch:
+    type: git
+    url: https://github.com/YOUR_NAME/YOUR_VEHICLE_launch.git
+    version: main
YOUR_VEHICLE_launch を Autoware 起動システムに適応させる
YOUR_VEHICLE_description で
車両記述パッケージで URDF とパラメータを定義します (例については、サンプル車両記述パッケージを参照してください)。

YOUR_VEHICLE_launch 時
起動ファイルを作成します (たとえば、サンプル車両起動パッケージを参照してください)。同じハードウェア設定の車両が複数ある場合は、vehicle_idそれらを区別するために指定できます。

YOUR_SENSOR_KIT_description を Autoware 起動システムに適合させる
YOUR_SENSOR_KIT_description にあります
ここですべてのセンサーの URDF および外部パラメーターを定義します (たとえば、サンプル センサー キットの説明パッケージを参照してください)。事前にすべてのセンサーの外部パラメーターを調整する必要があることに注意してください。

YOUR_SENSOR_KIT_launch 時
launch/sensing.launch.xml車両上のすべてのセンサーのインターフェイスを起動する作成。(たとえば、サンプル センサー キットの起動パッケージを参照してください)。

!!! 注記

At this point, you are now able to run Autoware's Planning Simulator to do a basic test of your vehicle and sensing packages.
To do so, you need to build and install Autoware using your cloned repository. Follow the [steps for either Docker or source installation](../installation/) (starting from the dependency installation step) and then run the following command:

```bash
ros2 launch autoware_launch planning_simulator.launch.xml vehicle_model:=YOUR_VEHICLE sensor_kit:=YOUR_SENSOR_KIT map_path:=/PATH/TO/YOUR/MAP
```
次に、車両と車両のセンサー構成を定義する記述パッケージを作成する必要があります。

次の2つのパッケージを作成します:

- YOUR_VEHICLE_launch (例は[ここ](https://github.com/autowarefoundation/sample_vehicle_launch)を参照してください)
- YOUR_SENSOR_KIT_launch (例は[ここ](https://github.com/autowarefoundation/sample_sensor_kit_launch)を参照してください)

作成したら、クローンされたAutowareリポジトリの`autoware.repos`ファイルを更新して、これら2つの説明パッケージを参照する必要があります。

```diff
-  # sensor_kit
-  sensor_kit/sample_sensor_kit_launch:
-    type: git
-    url: https://github.com/autowarefoundation/sample_sensor_kit_launch.git
-    version: main
-  # vehicle
-  vehicle/sample_vehicle_launch:
-    type: git
-    url: https://github.com/autowarefoundation/sample_vehicle_launch.git
-    version: main
+  # sensor_kit
+  sensor_kit/YOUR_SENSOR_KIT_launch:
+    type: git
+    url: https://github.com/YOUR_NAME/YOUR_SENSOR_KIT_launch.git
+    version: main
+  # vehicle
+  vehicle/YOUR_VEHICLE_launch:
+    type: git
+    url: https://github.com/YOUR_NAME/YOUR_VEHICLE_launch.git
+    version: main
```

### YOUR_VEHICLE_launchをAutoware起動システムに適応させる

#### YOUR_VEHICLE_descriptionで

車両記述パッケージで URDF とパラメータを定義します(例については[車両記述パッケージのサンプル](https://github.com/autowarefoundation/sample_vehicle_launch/tree/main/sample_vehicle_description)を参照してください)。

#### YOUR_VEHICLE_launchで

起動ファイルを作成します(例については[車両起動パッケージのサンプル](https://github.com/autowarefoundation/sample_vehicle_launch/tree/main/sample_vehicle_launch)を参照してください)。
同じハードウェア設定の車両が複数ある場合は`vehicle_id`をそれらを区別するために指定できます。

### Adapt YOUR_SENSOR_KIT_description for autoware launching system

#### YOUR_SENSOR_KIT_descriptionで

Define URDF and extrinsic parameters for all the sensors here (refer to the [sample sensor kit description package](https://github.com/autowarefoundation/sample_sensor_kit_launch/tree/main/sample_sensor_kit_description) for example).
Note that you need to calibrate extrinsic parameters for all the sensors beforehand.

#### YOUR_SENSOR_KIT_launchで

Create `launch/sensing.launch.xml` that launches the interfaces of all the sensors on the vehicle. (refer to the [sample sensor kit launch package](https://github.com/autowarefoundation/sample_sensor_kit_launch/tree/main/sample_sensor_kit_launch) for example).

!!! 注記

    この時点で、Autowareの計画シミュレーターを実行して、車両と計測パッケージの基本テストを実行できるようになりました。
    これを行うには複製したリポジトリを使用してAutowareを構築してインストールする必要があります。[Dockerまたはソースインストール手順](../installation/) (依存関係のインストール手順から開始)に従い、次のコマンドを実行します:

    ```bash
    ros2 launch autoware_launch planning_simulator.launch.xml vehicle_model:=YOUR_VEHICLE sensor_kit:=YOUR_SENSOR_KIT map_path:=/PATH/TO/YOUR/MAP
    ```

## 3. `vehicle_interface`パッケージを作成する

1. `vehicle_cmd_gate`からのコマンドメッセージを受信し、それに応じて車両を運転します
2. 車両ステータス情報をAutowareに送信する

`vehicle_interface`パッケージの要件に関する詳細情報は[Vehicle Interface設計ドキュメント](../../design/autoware-interfaces/components/vehicle-interface.md)で見つけることができます。
車両インターフェイス パッケージの例としてTIER IVの[pacmod_interfaceリポジトリ](https://github.com/tier4/pacmod_interface)を参照することもできます。

## 4. マップの作成

Autowareを使用するには、点群マップとベクターマップの両方が必要です。
マップデザインの詳細については[ここ](../../design/autoware-architecture/map/index.md)をクリックしてください。

### 点群マップを作成する

LiDARベースのSLAM (Simultaneous Localization And Mapping) パッケージなどのサードパーティツールを使用して`.pcd`形式で点群マップを作成します。
詳細については[ここ](creating-maps/index.md)をクリックしてください。

### ベクターマップを作成する

[TIER IV's Vector Map Builder](https://tools.tier4.jp/)などのサードパーティツールを使用して、Lanelet2 形式の`.osm`ファイルを作成します。

## 5. オートウェアを起動する

This section briefly explains how to run your vehicle with Autoware.

### Autowareのインストール

[Autowareのインストール手順](../../installation/)に従います。

### Autowareを起動する

次のコマンドでAutowareを起動します。:

```bash
ros2 launch autoware_launch autoware.launch.xml vehicle_model:=YOUR_VEHICLE sensor_kit:=YOUR_SENSOR_KIT map_path:=/PATH/TO/YOUR/MAP
```

### 初期ポーズを設定する

GNSSが利用可能な場合、Autowareは車両の姿勢を自動的に初期化します。

そうでない場合はRViz GUIを使用して初期ポーズを設定する必要があります。

1. ツールバーの 2Dポーズ推定ボタンをクリックするか、Pキーを押します。
2. 3Dビューペインで、マウスの左ボタンをクリックしたままドラッグして、初期ポーズの方向を設定します。

### ゴールポーズを設定する

自己車両のゴールポーズを設定します。

1. ツールバーの2Dナビゲーション目標ボタンをクリックするか、Gキーを押します。
2. 3Dビューペインで、マウスの左ボタンをクリックしたままドラッグして、ゴールポーズの方向を設定します。
   成功すると、計算された計画パスがRViz上に表示されます。

### 噛み合わせる

ターミナルで次のコマンドを実行します。

```bash
source ~/autoware.YOURS/install/setup.bash
ros2 topic pub /autoware.YOURS/engage autoware_auto_vehicle_msgs/msg/Engage "engage: true" -1
```

RViz経由で"AutowareStatePanel"を使用することもできます。
パネルは`Panels > Add New Panel > tier4_state_rviz_plugin > AutowareStatePanel`にあります。

![Autoware State Panel](images/autoware-state-panel.png){: style="height:360px;width:640px"}

これで、車両は計算された経路に沿って走行するはずです。!

## 6. 車両と環境に合わせてパラメータを調整する

車両を運用する領域に応じてパラメータを調整する必要がある場合があります。
