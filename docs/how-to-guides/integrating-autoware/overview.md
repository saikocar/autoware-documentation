概要
要件: 実車のハードウェアを準備する
車両の前提条件:

Autoware インストールの前提条件を満たすオンボード コンピューター
以下のデバイスが接続されています
ドライブバイワイヤインターフェース
ライダー
オプション：慣性計測ユニット
オプション: カメラ
オプション: GNSS
1. Autoware メタリポジトリの作成
Autoware メタ リポジトリを作成します。簡単な方法の 1 つは、autowarefoundation/autoware をフォークしてクローンを作成することです。リポジトリをフォークする方法については、GitHub Docsを参照してください。

git clone https://github.com/YOUR_NAME/autoware.git
複数の種類の車両を設定する場合は、「autoware.vehicle_A」または「autoware.vehicle_B」のようなサフィックスを追加することをお勧めします。

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
3.vehicle_interfaceパッケージを作成する
車両用のインターフェース パッケージを作成する必要があります。このパッケージでは以下の2つの機能を提供する予定です。

からのコマンドメッセージを受信しvehicle_cmd_gate、それに応じて車両を運転します
車両ステータス情報を Autoware に送信する
パッケージの要件に関する詳細情報は、Vehicle Interface 設計ドキュメントvehicle_interfaceで見つけることができます。車両インターフェイス パッケージの例として、 TIER IV のpacmod_interface リポジトリを参照することもできます。

4. マップの作成
Autoware を使用するには、点群マップとベクトル マップの両方が必要です。マップデザインの詳細については、ここをクリックしてください。

点群マップを作成する
LiDAR ベースの SLAM (Simultaneous Localization And Mapping) パッケージなどのサードパーティ ツールを使用して、次の形式で点群マップを作成します.pcd。詳細については、ここをクリックしてください。

ベクトルマップを作成する
TIER IV の Vector Map Builderなどのサードパーティ ツールを使用して、Lanelet2 形式の.osmファイルを作成します。

5. オートウェアを起動する
このセクションでは、Autoware を使用して車両を実行する方法について簡単に説明します。

オートウェアのインストール
Autoware のインストール手順に従います。

オートウェアを起動する
次のコマンドで Autoware を起動します。

ros2 launch autoware_launch autoware.launch.xml vehicle_model:=YOUR_VEHICLE sensor_kit:=YOUR_SENSOR_KIT map_path:=/PATH/TO/YOUR/MAP
初期ポーズを設定する
GNSS が利用可能な場合、Autoware は車両の姿勢を自動的に初期化します。

そうでない場合は、RViz GUI を使用して初期ポーズを設定する必要があります。

ツールバーの 2D ポーズ推定ボタンをクリックするか、P キーを押します。
3D ビュー ペインで、マウスの左ボタンをクリックしたままドラッグして、初期ポーズの方向を設定します。
ゴールポーズを設定する
自我車両のゴールポーズを設定します。

ツールバーの「2D ナビゲーション目標」ボタンをクリックするか、G キーを押します。
3D ビュー ペインで、マウスの左ボタンをクリックしたままドラッグして、ゴール ポーズの方向を設定します。成功すると、計算された計画パスが RViz 上に表示されます。
従事する
ターミナルで次のコマンドを実行します。

source ~/autoware.YOURS/install/setup.bash
ros2 topic pub /autoware.YOURS/engage autoware_auto_vehicle_msgs/msg/Engage "engage: true" -1
RViz 経由で「AutowareStatePanel」を使用することもできます。パネルは にありますPanels > Add New Panel > tier4_state_rviz_plugin > AutowareStatePanel。

Autoware 状態パネル{: style="高さ:360px;幅:640px"}

これで、車両は計算された経路に沿って走行するはずです。

6. 車両と環境に合わせてパラメータを調整する
車両を運用する領域に応じてパラメータを調整する必要がある場合があります。
# Overview

## Requirement: prepare your real vehicle hardware

Prerequisites for the vehicle:

- An onboard computer that satisfies the [Autoware installation prerequisites](../../installation/autoware/source-installation.md#prerequisites)
- The following devices attached
  - Drive-by-wire interface
  - LiDAR
  - Optional: Inertial measurement unit
  - Optional: Camera
  - Optional: GNSS

## 1. Creating your Autoware meta-repository

Create your Autoware meta-repository.
One easy way is to fork [autowarefoundation/autoware](https://github.com/autowarefoundation/autoware) and clone it.
For how to fork a repository, refer to [GitHub Docs](https://docs.github.com/en/get-started/quickstart/fork-a-repo).

```bash
git clone https://github.com/YOUR_NAME/autoware.git
```

If you set up multiple types of vehicles, adding a suffix like "autoware.vehicle_A" or "autoware.vehicle_B" is recommended.

## 2. Creating the your vehicle and sensor description

Next, you need to create description packages that define the vehicle and sensor configuration of your vehicle.

Create the following two packages:

- YOUR_VEHICLE_launch (see [here](https://github.com/autowarefoundation/sample_vehicle_launch) for example)
- YOUR_SENSOR_KIT_launch (see [here](https://github.com/autowarefoundation/sample_sensor_kit_launch) for example)

Once created, you need to update the `autoware.repos` file of your cloned Autoware repository to refer to these two description packages.

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

### Adapt YOUR_VEHICLE_launch for autoware launching system

#### At YOUR_VEHICLE_description

Define URDF and parameters in the vehicle description package (refer to the [sample vehicle description package](https://github.com/autowarefoundation/sample_vehicle_launch/tree/main/sample_vehicle_description) for an example).

#### At YOUR_VEHICLE_launch

Create a launch file (refer to the [sample vehicle launch package](https://github.com/autowarefoundation/sample_vehicle_launch/tree/main/sample_vehicle_launch) for example).
If you have multiple vehicles with the same hardware setup, you can specify `vehicle_id` to distinguish them.

### Adapt YOUR_SENSOR_KIT_description for autoware launching system

#### At YOUR_SENSOR_KIT_description

Define URDF and extrinsic parameters for all the sensors here (refer to the [sample sensor kit description package](https://github.com/autowarefoundation/sample_sensor_kit_launch/tree/main/sample_sensor_kit_description) for example).
Note that you need to calibrate extrinsic parameters for all the sensors beforehand.

#### At YOUR_SENSOR_KIT_launch

Create `launch/sensing.launch.xml` that launches the interfaces of all the sensors on the vehicle. (refer to the [sample sensor kit launch package](https://github.com/autowarefoundation/sample_sensor_kit_launch/tree/main/sample_sensor_kit_launch) for example).

!!! note

    At this point, you are now able to run Autoware's Planning Simulator to do a basic test of your vehicle and sensing packages.
    To do so, you need to build and install Autoware using your cloned repository. Follow the [steps for either Docker or source installation](../installation/) (starting from the dependency installation step) and then run the following command:

    ```bash
    ros2 launch autoware_launch planning_simulator.launch.xml vehicle_model:=YOUR_VEHICLE sensor_kit:=YOUR_SENSOR_KIT map_path:=/PATH/TO/YOUR/MAP
    ```

## 3. Create a `vehicle_interface` package

You need to create an interface package for your vehicle.
The package is expected to provide the following two functions.

1. Receive command messages from `vehicle_cmd_gate` and drive the vehicle accordingly
2. Send vehicle status information to Autoware

You can find detailed information about the requirements of the `vehicle_interface` package in the [Vehicle Interface design documentation](../../design/autoware-interfaces/components/vehicle-interface.md).
You can also refer to TIER IV's [pacmod_interface repository](https://github.com/tier4/pacmod_interface) as an example of a vehicle interface package.

## 4. Create maps

You need both a pointcloud map and a vector map in order to use Autoware.
For more information on map design, please click [here](../../design/autoware-architecture/map/index.md).

### Create a pointcloud map

Use third-party tools such as a LiDAR-based SLAM (Simultaneous Localization And Mapping) package to create a pointcloud map in the `.pcd` format.
For more information, please click [here](creating-maps/index.md).

### Create vector map

Use third-party tools such as [TIER IV's Vector Map Builder](https://tools.tier4.jp/) to create a Lanelet2 format `.osm` file.

## 5. Launch Autoware

This section briefly explains how to run your vehicle with Autoware.

### Install Autoware

Follow the [installation steps of Autoware](../../installation/).

### Launch Autoware

Launch Autoware with the following command:

```bash
ros2 launch autoware_launch autoware.launch.xml vehicle_model:=YOUR_VEHICLE sensor_kit:=YOUR_SENSOR_KIT map_path:=/PATH/TO/YOUR/MAP
```

### Set initial pose

If GNSS is available, Autoware automatically initializes the vehicle's pose.

If not, you need to set the initial pose using the RViz GUI.

1. Click the 2D Pose estimate button in the toolbar, or hit the P key
2. In the 3D View pane, click and hold the left mouse button, and then drag to set the direction for the initial pose.

### Set goal pose

Set a goal pose for the ego vehicle.

1. Click the 2D Nav Goal button in the toolbar, or hit the G key
2. In the 3D View pane, click and hold the left mouse button, and then drag to set the direction for the goal pose.
   If successful, you will see the calculated planning path on RViz.

### Engage

In your terminal, execute the following command.

```bash
source ~/autoware.YOURS/install/setup.bash
ros2 topic pub /autoware.YOURS/engage autoware_auto_vehicle_msgs/msg/Engage "engage: true" -1
```

You can also engage via RViz with "AutowareStatePanel".
The panel can be found in `Panels > Add New Panel > tier4_state_rviz_plugin > AutowareStatePanel`.

![Autoware State Panel](images/autoware-state-panel.png){: style="height:360px;width:640px"}

Now the vehicle should drive along the calculated path!

## 6. Tune parameters for your vehicle & environment

You may need to tune your parameters depending on the domain in which you will operate your vehicle.
