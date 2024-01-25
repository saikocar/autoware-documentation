# 車両インターフェースの作成

## 車両インターフェースの実装方法

次の手順では、車両インターフェイスを作成する方法について説明します。

### 1. 車両インターフェース用のディレクトリを作成します。

`<your-autoware-dir>/src/vehicle/external`に車両インターフェースを作成することをお勧めします。

```bash
cd <your-autoware-dir>/src/vehicle/external
```

### 2. 独自の車両インターフェースをインストールまたは実装する

すでに完全な車両インターフェイス パッケージ([`pacmod_interface`](https://github.com/tier4/pacmod_interface/tree/main)など)がある場合は、それを環境にインストールできます。
そうでない場合は、独自の車両インターフェイスを自分で実装する必要があります。
`ros2 pkg create`で新しいパッケージを作成しましょう。
次の例では、`my_vehicle_interface`という名前の車両インターフェイス パッケージを作成する方法を示します。

```bash
ros2 pkg create --build-type ament_cmake my_vehicle_interface
```

次に、車両インターフェースの実装を`my_vehicle_interface/src`に記述する必要があります。
繰り返しますが、この実装は車両の制御デバイスに非常に固有であるため、車両インターフェイスの実装方法を詳細に説明することはこのドキュメントの範囲を超えています。
考慮される可能性のあるいくつかの要因を次に示します:

- 車両を制御するために Autoware からの制御コマンド トピックの必要なトピックのサブスクリプションをいくつか示します:

| トピック名                           | トピックの種類                                             | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------ | ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /control/command/control_cmd         | autoware_auto_control_msgs/msg/AckermannControlCommand | このトピックには、ステアリング タイヤの角度、速度、加速度など、車両を制御するための主要なトピックが含まれています。                                                                                                                                                                                                                                                                                                                                                                                                               |
| /control/command/gear_cmd            | autoware_auto_vehicle_msgs/msg/GearCommand             | このトピックには自動運転用のギア コマンドが含まれています。ギアの値を理解するにはこの型の[メッセージ値](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/GearCommand.idl)を確認してください。                                                                                                                                                                                                                                             |
| /control/current_gate_mode           | tier4_control_msgs/msg/GateMode                        | このトピックでは、autowareの制御について説明します。詳細については、[GateMode](https://github.com/tier4/tier4_autoware_msgs/blob/tier4/universe/tier4_control_msgs/msg/GateMode.msg)メッセージ タイプを確認してください。                                                                                                                                                                                                                                                                                                       |
| /control/command/emergency_cmd       | tier4_vehicle_msgs/msg/VehicleEmergencyStamped         | このトピックでは、Autoware が緊急状態のときに緊急メッセージを送信します。詳細については、[VehicleEmergencyStamped](https://github.com/tier4/tier4_autoware_msgs/blob/tier4/universe/tier4_vehicle_msgs/msg/VehicleEmergencyStamped.msg)メッセージ タイプを確認してください。                                                                                                                                                                                                                                                              |
| /control/command/turn_indicators_cmd | autoware_auto_vehicle_msgs/msg/TurnIndicatorsCommand   | このトピックでは、自分の車両の方向指示器について説明します。詳細については、[TurnIndicatorsCommand](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/TurnIndicatorsCommand.idl)メッセージ タイプを確認してください。                                                                                                                                                                                                                                                                      |
| /control/command/hazard_lights_cmd   | autoware_auto_vehicle_msgs/msg/HazardLightsCommand     | このトピックでは、ハザード ライトのコマンドを送信します。[HazardLightsCommand](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/HazardLightsCommand.idl)コマンドを確認してください                                                                                                                                                                                                                                                                                                                              |
| /control/command/actuation_cmd       | tier4_vehicle_msgs/msg/ActuationCommandStamped         | このトピックは、[車両インターフェイス](./vehicle-interface.md)セクションで説明した TYPE B で車両の制御に`raw_vehicle_command_converter`を使用する場合に有効になります。 要約すると、車両でタイプ B を使用している場合、このトピックはアクセル、ブレーキ、ステアリング ホイール作動コマンドに含まれています。詳細については、[ActuationCommandStamped](https://github.com/tier4/tier4_autoware_msgs/blob/tier4/universe/tier4_vehicle_msgs/msg/ActuationCommandStamped.msg)メッセージ タイプを確認してください。 |
| 等                                 | 等                                                   | 等                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

- 車両インターフェイスから Autoware への車両ステータス トピックのいくつかの必要なトピックの公開:

| トピック名                             | トピックの種類                                          | 説明                                                                                                                                                                                                                                                                   |
| -------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /vehicle/status/battery_charge         | tier4_vehicle_msgs/msg/BatteryStatus                | このトピックにはバッテリーに関する情報が含まれています。詳細については、[BatteryStatus](https://github.com/tier4/tier4_autoware_msgs/blob/tier4/universe/tier4_vehicle_msgs/msg/BatteryStatus.msg)メッセージ タイプを確認してください。この値は、燃料レベルなどを説明するために使用できます。 |
| /vehicle/status/control_mode           | autoware_auto_vehicle_msgs/msg/ControlModeReport    | このトピックでは、車両の現在の制御モードについて説明します。詳細については、[ControlModeReport](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/ControlModeReport.idl)メッセージ タイプを確認してください。                           |
| /vehicle/status/gear_status            | autoware_auto_vehicle_msgs/msg/GearReport           | このトピックには、車両の現在のギア状態が含まれます。詳細については、[GearReport](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/GearReport.idl) メッセージ タイプを確認してください。                                      |
| /vehicle/status/hazard_lights_status   | autoware_auto_vehicle_msgs/msg/HazardLightsReport   | このトピックでは、車両のハザード ライトのステータスについて説明します。詳細については、[HazardLightsReport](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/HazardLightsReport.idl)メッセージ タイプを確認してください。                          |
| /vehicle/status/turn_indicators_status | autoware_auto_vehicle_msgs/msg/TurnIndicatorsReport | このトピックでは、車両のステアリング ステータスを報告します。詳細については、[SteeringReport](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/SteeringReport.idl)メッセージ タイプを確認してください。                                   |
| /vehicle/status/steering_status        | autoware_auto_vehicle_msgs/msg/SteeringReport       | このトピックでは、車両のステアリング ステータスを報告します。詳細については、[SteeringReport](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/SteeringReport.idl) メッセージ タイプを確認してください。                                    |
| /vehicle/status/velocity_Status        | autoware_auto_vehicle_msgs/msg/VelocityReport       | このトピックでは、車両の速度ステータスが表示されます。詳細については、[VelocityReport](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/VelocityReport.idl)メッセージ タイプを確認してください。                                   |
| 等                                   | 等                                                | 等                                                                                                                                                                                                                                                                          |

この図は、サンプル トピックとメッセージ タイプを説明する
車両インターフェイスとオートウェアの通信の例です。

<figure markdown>
  ![vehicle_communication](images/autoware-vehicle-communication.svg){ align=center }
  <figcaption>
    車両とautowareの通信のサンプル デモンストレーション。
    この図にはいくつかのトピックとタイプが含まれており、
    必要な制御コマンドやautowareの更新に変更できます。
  </figcaption>
</figure>

車両インターフェイス上でこれらのトピックを使用してサブスクライバーとパブリッシャーを作成する必要があります。
`/control/command/control_cmd`トピックのサブスクライブと`/vehicle/status/gear_status`トピックのパブリッシングの
簡単なデモを使って説明しましょう

したがって、YOUR-OWN-VEHICLE-INTERFACE.hpp`ヘッダー ファイルは次のようになります:

```c++
...
#include <autoware_auto_control_msgs/msg/ackermann_control_command.hpp>
#include <autoware_auto_vehicle_msgs/msg/gear_report.hpp>
...

class <YOUR-OWN-INTERFACE> : public rclcpp::Node
{
public:
    ...
private:
    ...
    // from autoware
    rclcpp::Subscription<autoware_auto_control_msgs::msg::AckermannControlCommand>::SharedPtr
    control_cmd_sub_;
    ...
    // from vehicle
    rclcpp::Publisher<autoware_auto_vehicle_msgs::msg::GearReport>::SharedPtr gear_status_pub_;
    ...
    // autoware command messages
    ...
    autoware_auto_control_msgs::msg::AckermannControlCommand::ConstSharedPtr control_cmd_ptr_;
    ...
    // callbacks
    ...
    void callback_control_cmd(
    const autoware_auto_control_msgs::msg::AckermannControlCommand::ConstSharedPtr msg);
    ...
    void to_vehicle();
    void from_vehicle();
}
```

`YOUR-OWN-VEHICLE-INTERFACE.cpp`.cppファイルは次のようになります:

```c++
#include <YOUR-OWN-VEHICLE-INTERFACE>/<YOUR-OWN-VEHICLE-INTERFACE>.hpp>
...

<YOUR-OWN-VEHICLE-INTERFACE>::<YOUR-OWN-VEHICLE-INTERFACE>()
: Node("<YOUR-OWN-VEHICLE-INTERFACE>")
{
  ...
  /* subscribers */
  using std::placeholders::_1;
  // from autoware
  control_cmd_sub_ = create_subscription<autoware_auto_control_msgs::msg::AckermannControlCommand>(
    "/control/command/control_cmd", 1, std::bind(&<YOUR-OWN-VEHICLE-INTERFACE>::callback_control_cmd, this, _1));
  ...
  // to autoware
  gear_status_pub_ = create_publisher<autoware_auto_vehicle_msgs::msg::GearReport>(
    "/vehicle/status/gear_status", rclcpp::QoS{1});
  ...
}

void <YOUR-OWN-VEHICLE-INTERFACE>::callback_control_cmd(
  const autoware_auto_control_msgs::msg::AckermannControlCommand::ConstSharedPtr msg)
{
  control_cmd_ptr_ = msg;
}

void <YOUR-OWN-VEHICLE-INTERFACE>::to_vehicle()
{
  ...
  // you should implement this structure according to your own vehicle design
  control_command_to_vehicle(control_cmd_ptr_);
  ...
}

void <YOUR-OWN-VEHICLE-INTERFACE>::to_autoware()
{
  ...
  // you should implement this structure according to your own vehicle design
  autoware_auto_vehicle_msgs::msg::GearReport gear_report_msg;
  convert_gear_status_to_autoware_msg(gear_report_msg);
  gear_status_pub_->publish(gear_report_msg);
  ...
}

```

- 必要に応じて制御値を変更します
  - 場合によっては、制御コマンドの変更が必要になる場合があります。たとえば、Autoware は車両速度情報を m/s 単位で要求しますが、車両が別の形式 (km/h など) で公開する場合は、Autoware に送信する前に変換する必要があります。

### 3. 起動ファイルを準備する

車両インターフェースを実装した後、または車両インターフェースを起動してデバッグしたい場合は、
車両インターフェースの起動ファイルを作成し、
それを[車両とセンサーの説明ページの作成時](../creating-vehicle-and-sensor-description/creating-vehicle-and-sensor-description.md)に
フォークして作成した`<VEHICLE_ID>_vehicle_launch`パッケージに含まれる
`vehicle_interface.launch.xml`に含めます

混乱しないでください。まず、独自の車両インターフェース モジュール ((`my_vehicle_interface.launch.xml`のような)の起動ファイルを作成し、 **それを別のディレクトリにある`vehicle_interface.launch.xml`に含める必要があります。** 詳細は次のとおりです。

1. `my_vehicle_interface`ディレクトリに`launch`を追加し、その中に独自の車両インターフェースの起動ファイルを作成します。ROS 2 ドキュメントの[起動ファイルの作成](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Launch-Main.html)を参照してください。

2. vehicle_interface用に作成された起動ファイルを`<YOUR-VEHICLE-NAME>_launch/<YOUR-VEHICLE-NAME>_launch/launch/vehicle_interface.launch.xml`を開いてインクルードするために、以下のようにインクルード行を追加します。

```xml title="vehicle_interface.launch.xml"
<?xml version="1.0" encoding="UTF-8"?>
<launch>
    <arg name="vehicle_id" default="$(env VEHICLE_ID default)"/>
    <!-- please add your created vehicle interface launch file -->
    <include file="$(find-pkg-share my_vehicle_interface)/launch/my_vehicle_interface.launch.xml">
    </include>
</launch>
```

最終的に、ディレクトリ構造は以下のようになります。
わかりやすくするためにほとんどのファイルは省略されていますが、ここに示されているファイルは、以前のプロセスと現在のプロセスで述べたように変更する必要があります。

```diff
<your-autoware-dir>/
└─ src/
    └─ vehicle/
        ├─ external/
+       │   └─ <YOUR-VEHICLE-NAME>_interface/
+       │       ├─ src/
+       │       └─ launch/
+       │            └─ my_vehicle_interface.launch.xml
+       └─ <YOUR-VEHICLE-NAME>_launch/ (COPIED FROM sample_vehicle_launch)
+           ├─ <YOUR-VEHICLE-NAME>_launch/
+           │  ├─ launch/
+           │  │  └─ vehicle_interface.launch.xml
+           │  ├─ CMakeLists.txt
+           │  └─ package.xml
+           ├─ <YOUR-VEHICLE-NAME>_description/
+           │  ├─ config/
+           │  ├─ mesh/
+           │  ├─ urdf/
+           │  │  └─ vehicle.xacro
+           │  ├─ CMakeLists.txt
+           │  └─ package.xml
+           └─ README.md
```

### 4. 車両インターフェース パッケージと起動パッケージを構築する

`my_vehicle_interface`、`<YOUR-VEHICLE-NAME>_launch`、
`<YOUR-VEHICLE-NAME>_description`の3つのパッケージを`colcon build`でビルドします、
あるいは他の作業を行っている場合は、Autoware 全体をビルドすることもできます。

```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-select my_vehicle_interface <YOUR-VEHICLE-NAME>_launch <YOUR-VEHICLE-NAME>_description
```

### 5. Autoware を起動するとき

最後に、車両インターフェース モジュールの実装が完了しました。以下の例のように、適切な`vehicle_model`オプションを指定して Autoware を起動する必要があることに注意してください。この例では計画シミュレーターを起動しています。

```bash
ros2 launch autoware_launch planning.launch.xml map_path:=$HOME/autoware_map/sample-map-planning vehicle_model:=<YOUR-VEHICLE-NAME> sensor_model:=<YOUR-VEHICLE-NAME>_sensor_kit
```

### チップ

役立つヒントがいくつかあります。

- 必要に応じて、車両インターフェースをより小さなパッケージに分割できます。その場合、ディレクトリ構造は以下のようになります (ただし、これが唯一の方法ではありません)。`my_vehicle_interface.launch.xml`ですべてのパッケージを起動することを忘れないでください。

  ```diff
  <your-autoware-dir>/
  └─ src/
      └─ vehicle/
          ├─ external/
          │   └─ my_vehicle_interface/
          │       ├─ src/
          │       │   ├─ package1/
          │       │   ├─ package2/
          │       │   └─ package3/
          │       └─ launch/
          │            └─ my_vehicle_interface.launch.xml
          ├─ sample_vehicle_launch/
          └─ my_vehicle_name_launch/
  ```

- 車両インターフェイスを使用し、開いている git リポジトリからパッケージを起動する場合、または独自の git リポジトリを作成した場合は、以下の例のように、autoware フォルダーの直下にある`autoware.repos`フォルダーの直下にあるファイルにそれらのリポジトリを追加することを強くお勧めします。バージョンタグでブランチまたはコミットのハッシュを指定できます。

  ```yaml title="autoware.repos"
  # vehicle (this section should be somewhere in autoware.repos and add the below)
  vehicle/external/my_vehicle_interface:
    type: git
    url: https://github.com/<repository-name-B>/my_vehicle_interface.git
    version: main
  ```

  その後、`vcs import`コマンドを使用して、環境全体を別のローカル デバイスに簡単にインポートできます。([ソースインストールガイド](https://autowarefoundation.github.io/autoware-documentation/main/installation/autoware/source-installation/#how-to-set-up-a-workspace)を確認してください)
