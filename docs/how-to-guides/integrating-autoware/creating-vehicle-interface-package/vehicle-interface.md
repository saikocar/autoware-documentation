車両インターフェース
車両インターフェースとは何ですか?
車両インターフェイス パッケージの目的は、オートウェアから計算された制御メッセージを車両が理解できる形式に変換して車両に送信し (CAN、シリアル メッセージなど)、車両から送信されるデータをデコードして、オートウェアが期待する ROS 2 トピックを通じて公開します。したがって、 vehicle_interface の目的は次のとおりであると言えます。

Autoware 制御コマンドを車両固有の形式に変換します。Autoware の制御コマンドの例:

ラテラルコントロール: ステアリングタイヤ角度、ステアリングタイヤ回転速度
縦方向のコントロール: 速度、加速度、ジャーク
車両固有の形式の車両ステータス情報を Autoware メッセージに変換します。

Autoware は、次のような制御コマンドを公開しています。

速度制御
ステアリング制御
車のライトコマンド
等
次に、車両インターフェイスはこれらのコマンドを次のような作動に変換します。

モーターとブレーキの作動
ハンドル操作
照明制御
等
したがって、車両インターフェイスは、Autoware によって提供される入力コマンドを実現するために車両の制御デバイスを実行するモジュールと考えてください。

![vehicle_interface_IO](images/Vehicle-Interface-Bus-ODD-Architecture.drawio.svg){ align=center } 車両インターフェースの入出力の例
自分の車両を制御するためのインターフェイスには 2 種類があります。

ターゲットステアリングとターゲット速度/加速度インターフェイス。(本書ではタイプAとします)
一般化されたターゲット コマンド インターフェイス (つまり、アクセル/ブレーキ ペダル、ステアリング トルク)。(本書ではタイプBとします)
タイプ A の場合、制御インターフェイスには目標ステアリングと目標速度/加速度が含まれます。

したがって、このタイプでは、任意の速度または加速度によって速度制御が行われます。
ステアリング制御は、所望のステアリング角度および/またはステアリング角度速度によって行われます。
一方、タイプ B は、一般化されたターゲット コマンド インターフェイス (アクセル/ブレーキ ペダル、ステアリング トルクなど) を特徴とし、より動的で適応性のある制御スキームを導入します。この構成では、車両は、 の出力であるオートウェア作動コマンドから直接入力するように制御されraw_vehicle_cmd_converter、より直感的で人間に近い運転体験が可能になります。

自分の車両をこのように使用する場合 (つまり、アクセルとブレーキ ペダルを制御する)、raw_vehicle_cmd_converterパッケージを使用して autoware 出力制御 cmd をブレーキ、アクセル、ステアリング マップに変換する必要があります。そのためには、ブレーキ、ガソリン、ステアリングの調整が必要になります。したがって、accel_brake_map_calibratorパッケージを使用してキャリブレーションを取得できます。車両の作動を調整する手順に従ってください。

これらの制御インターフェイスの選択は、設計と開発のプロセスに大きな影響を与えます。タイプ A を使用する予定の場合は、ドライブ バイ ワイヤ システムで速度または加速度を制御します。タイプ B の方が実装に適している場合は、車両のブレーキ ペダルとアクセル ペダルを制御する必要があります。

車両インターフェースと車両の制御デバイス間の通信
車両で Autoware を使用して運転することを計画している場合、車両はいくつかの要件を満たしている必要があります。
タイプA	タイプB
車両は、目標速度または加速度によって前後方向に制御できます。	車両は、特定のターゲット コマンドによって前後方向に制御できます。(つまり、ブレーキとアクセルペダル)
目標ステアリング角度によって車両を横方向に制御できます。(オプションでステアリングレートも使用できます)	車両は、特定のターゲット コマンドによって横方向に制御できます。（操舵トルク）
車両は、上記の車両ステータスのトピックで説明されている速度または加速度の情報とステアリング情報を提供する必要があります。	車両は、上記の車両ステータスのトピックで説明されている速度または加速度の情報とステアリング情報を提供する必要があります。
また、横方向は操舵角で制御し、前後方向は特定の目標で制御したい場合など、タイプAとタイプBを混合して使用することもできます。これを行うには、ステアリング情報を取得するためにサブスクライブし/control/command/control_cmd、/control/command/actuation_cmdアクセル ペダルとブレーキ ペダルの作動コマンドをサブスクライブする必要があります。次に、これらのメッセージを独自の vehicle_interface 設計で処理する必要があります。
Autoware で実行するには、車両の低レベル コントローラーを採用する必要があります。
車両はターゲット シフト モードで制御できますが、Autoware にシフト情報を提供する必要もあります。
車両で CAN 通信を使用している場合は、 Autoware のros2_socketcanパッケージを参照してください。
車両でシリアル通信を使用している場合は、serial-portを確認できます。
# Vehicle interface

## What is the vehicle interface?

The purpose of the vehicle interface package is to convert the control messages calculated from the autoware into a form that the vehicle can understand and transmit them to the vehicle (CAN, Serial message, etc.) and to decode the data coming from the vehicle and publish it through the ROS 2 topics that the autoware expects.
So, we can say that the purposes of the vehicle_interface are:

1. Converting Autoware control commands to a vehicle-specific format. Example control commands of autoware:

   - lateral controls: steering tire angle, steering tire rotation rate
   - longitudinal controls: speed, acceleration, jerk

2. Converting vehicle status information in a vehicle-specific format to Autoware messages.

Autoware publishes control commands such as:

- Velocity control
- Steering control
- Car light commands
- etc.

Then, the vehicle interface converts these commands into actuation such like:

- Motor and brake activation
- Steering-wheel operation
- Lighting control
- etc.

So think of the vehicle interface as a module that runs the vehicle's control device to realize the input commands provided by Autoware.

<figure markdown>
  ![vehicle_interface_IO](images/Vehicle-Interface-Bus-ODD-Architecture.drawio.svg){ align=center }
  <figcaption>
    An example of inputs and outputs for vehicle interface
  </figcaption>
</figure>

There are two types of interfaces for controlling your own vehicle:

1. Target steering and target velocity/acceleration interface. (It will be named as Type A for this document)
2. Generalized target command interface (i.e., accel/brake pedal, steering torque). (It will be named as Type B for this document)

For Type A,
where the control interface encompasses target steering and target velocity/acceleration.

- So, at this type, speed control is occurred by desired velocity or acceleration.
- Steering control is occurred by desired steering angle and/or steering angle velocity.

On the other hand, Type B, characterized by a generalized target command interface (e.g., accel/brake pedal, steering torque),
introduces a more dynamic and adaptable control scheme.
In this configuration,
the vehicle
controlled to direct input from autoware actuation commands which output of the `raw_vehicle_cmd_converter`,
allowing for a more intuitive and human-like driving experience.

If you use your own vehicle like this way
(i.e., controlling gas and brake pedal),
you need
to use [raw_vehicle_cmd_converter](https://github.com/autowarefoundation/autoware.universe/tree/main/vehicle/raw_vehicle_cmd_converter) package
to convert autoware output control cmd to brake, gas and steer map.
In order to do that, you will need brake, gas and steering calibration.
So,
you can get the calibration with using [accel_brake_map_calibrator](https://github.com/autowarefoundation/autoware.universe/tree/main/vehicle/accel_brake_map_calibrator/accel_brake_map_calibrator) package.
Please follow the steps for calibration your vehicle actuation.

The choice between these control interfaces profoundly influences the design and development process.
If you are planning to use Type A,
then you will control velocity or acceleration over drive by-wire systems.
If type B is more suitable for your implementation,
then you will need to control your vehicle's brake and gas pedal.

### Communication between the vehicle interface and your vehicle's control device

- If you are planning to drive by autoware with your vehicle, then your vehicle must satisfy some requirements:

| Type A                                                                                                                                         | Type B                                                                                                                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Your vehicle can be controlled in longitudinal direction by the target velocity or acceleration.                                               | Your vehicle can be controlled in longitudinal direction by the specific target commands. (i.e., brake and gas pedal)                          |
| Your vehicle can be controlled in lateral direction by the target steering angle. (Optionally, you can use steering rate as well)              | Your vehicle can be controlled in lateral direction by the specific target commands. (i.e., steering torque)                                   |
| Your vehicle must provide velocity or acceleration information and steering information which is described at the vehicle status topics above. | Your vehicle must provide velocity or acceleration information and steering information which is described at the vehicle status topics above. |

- You can also use mixed Type A and Type B, for example, you want to use lateral controlling with a steering angle and longitudinal controlling with specific targets. In order to do that, you must subscribe `/control/command/control_cmd` for getting steering information, and you must subscribe `/control/command/actuation_cmd` for gas and brake pedal actuation commands. Then, you must handle these messages on your own vehicle_interface design.
- You must adopt your vehicle low-level controller to run with autoware.
- Your vehicle can be controlled by the target shift mode and also needs to provide Autoware with shift information.
- If you are using CAN communication on your vehicle, please refer to [ros2_socketcan](https://github.com/autowarefoundation/ros2_socketcan) package by Autoware.
- If you are using Serial communication on your vehicle, you can look at [serial-port](https://github.com/fedetft/serial-port/tree/master/3_async).
