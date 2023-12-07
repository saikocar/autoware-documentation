デフ車のカスタマイズ
1. はじめに
現在、Autoware は、車両がアッカーマン ステアリングを備えたアッカーマン運動学モデルを使用していることを前提としています。したがって、Autoware は、Control モジュールの出力に Ackermann コマンド形式を採用します ( Ackermann コマンドの概要についてはAckermannDrive ROS メッセージ定義を、 詳細については Autoware で使用されるAckermannControlCommands 構造体を参照してください)。

ただし、小型移動ロボットで一般的に使用される差動駆動運動学モデルに従う車両と Autoware を統合することは可能です。以下の図に示すように、ディファレンシャル車両は 4 輪または 2 輪のいずれかになります。

![Difference_vehicle](images/Difference_vehicle.svg){ align=center } 4 輪モデルと 2 輪モデルの差動車両のサンプル。
2. 手順
Autoware を差動駆動車両で使用する簡単な方法の 1 つは、vehicle_interfaceアッカーマン コマンドを差動駆動コマンドに変換するパッケージを作成することです。考慮する必要がある点は次の 2 つです。

vehicle_interfaceデフ車用パッケージの作成
適切な値を設定しますwheel_base
2.1vehicle_interfaceデフ車用パッケージの作成
Autoware の Ackermann コマンドは、次の 2 つの主要な制御入力で構成されます。

ステアリング角度 (
）
速度 (
）
逆に、一般的な差動駆動コマンドは次の入力で構成されます。

左車輪速度 (
）
右輪速度 (
）
したがって、アッカーマン コマンドを差動ドライブ コマンドに変換する 1 つの方法は、次の方程式を使用することです。

 
 

どこ
車輪の踏面を指します。

以下は、.cppアッカーマン モデルの運動学を差分モデルに変換するためのスニペットの例です。

...
void convert_ackermann_to_differential(
  autoware_auto_control_msgs::msg::AckermannControlCommand & ackermann_msg
  my_vehicle_msgs::msg::DifferentialCommand & differential_command)
{
    differential_command.left_wheel.velocity =
      ackermann_msg.longitudinal.speed - (ackermann_msg.lateral.steering_tire_angle * my_wheel_tread) / 2;
    differential_command.right_wheel.velocity =
      ackermann_msg.longitudinal.speed + (ackermann_msg.lateral.steering_tire_angle * my_wheel_tread) / 2;
}
...
パッケージを作成するときに考慮する必要があるその他の要素については、 「作成ページ」vehicle_interfaceを参照してください。vehicle_interface

2.2 適切な設定wheel_base
差動駆動ロボットには必ずしも前輪と後輪があるわけではないため、ホイールベース（前輪と後輪の車軸間の水平距離）を定義することができません。ただし、Autoware は何らかの値がwheel_base設定されることを期待しています。vehicle_info.param.yamlしたがって、 に疑似値を設定する必要がありますwheel_base。

の適切な疑似値は、wheel_base車両のサイズによって異なります。考えられる選択肢の 1 つは、 と同じ値に設定することですwheel_tread。

!!! 警告

- If the `wheel_base` value is set too small then the vehicle may behave unexpectedly. For example, the vehicle may drive beyond the bounds of a calculated path.
- Conversely, if `wheel_base` is set too large, the vehicle's range of motion will be restricted. The reason being that Autoware's Planning module will calculate an overly conservative trajectory based on the assumed vehicle length.
3. 既知の問題
モーションモデルの非互換性
Autoware は車両がステアリング システムを使用することを前提としているため、差動駆動システムの運動モデルの柔軟性を利用することはできません。

たとえば、freespace_plannerモジュールを使用して駐車操作を計画する場合、純粋な回転運動を使用する単純な軌道で車両を駐車できる場合でも、Autoware は差動駆動車両を前後に駆動する場合があります。
# Customizing for differential drive vehicle

## 1. Introduction

Currently, Autoware assumes that vehicles use an Ackermann kinematic model with Ackermann steering.
Thus, Autoware adopts the Ackermann command format for the Control module's output
(see [the AckermannDrive ROS message definition](http://docs.ros.org/en/api/ackermann_msgs/html/msg/AckermannDrive.html) for an overview of Ackermann commands,
and [the AckermannControlCommands struct](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/AckermannControlCommand.idl)
used in Autoware for more details).

However,
it is possible to integrate Autoware with a vehicle
that follows a differential drive kinematic model,
as commonly used by small mobile robots.
The differential vehicles can be either four-wheel or two-wheel, as described in the figure below.

<figure markdown>
  ![differential_vehicle](images/differential_vehicle.svg){ align=center }
  <figcaption>
    Sample differential vehicles with four-wheel and two-wheel models.
  </figcaption>
</figure>

## 2. Procedure

One simple way of using Autoware with a differential drive vehicle is to create a `vehicle_interface` package that translates Ackermann commands to differential drive commands.
Here are two points that you need to consider:

- Create `vehicle_interface` package for differential drive vehicle
- Set an appropriate `wheel_base`

### 2.1 Create a `vehicle_interface` package for differential drive vehicle

An Ackermann command in Autoware consists of two main control inputs:

- steering angle ($\omega$)
- velocity ($v$)

Conversely, a typical differential drive command consists of the following inputs:

- left wheel velocity ($v_l$)
- right wheel velocity ($v_r$)

So, one way in which an Ackermann command can be converted to a differential drive command is by using the following equations:

$$
v_l = v - \frac{l\omega}{2},
v_r = v + \frac{l\omega}{2}
$$

where $l$ denotes wheel tread.

Here is the example `.cpp` snippet
for converting ackermann model kinematics to a differential model:

```c++
...
void convert_ackermann_to_differential(
  autoware_auto_control_msgs::msg::AckermannControlCommand & ackermann_msg
  my_vehicle_msgs::msg::DifferentialCommand & differential_command)
{
    differential_command.left_wheel.velocity =
      ackermann_msg.longitudinal.speed - (ackermann_msg.lateral.steering_tire_angle * my_wheel_tread) / 2;
    differential_command.right_wheel.velocity =
      ackermann_msg.longitudinal.speed + (ackermann_msg.lateral.steering_tire_angle * my_wheel_tread) / 2;
}
...
```

For information about other factors
that need to be considered when creating a `vehicle_interface` package,
refer to the [creating `vehicle_interface` page](./creating-vehicle-interface.md).

### 2.2 Set an appropriate `wheel_base`

A differential drive robot does not necessarily have front and rear wheels, which means that the wheelbase (the horizontal distance between the axles of the front and rear wheels) cannot be defined. However, Autoware expects `wheel_base` to be set in `vehicle_info.param.yaml` with some value.
Thus, you need to set a pseudo value for `wheel_base`.

The appropriate pseudo value for `wheel_base` depends on the size of your vehicle.
Setting it to be the same value as `wheel_tread` is one possible choice.

!!! warning

    - If the `wheel_base` value is set too small then the vehicle may behave unexpectedly. For example, the vehicle may drive beyond the bounds of a calculated path.
    - Conversely, if `wheel_base` is set too large, the vehicle's range of motion will be restricted. The reason being that Autoware's Planning module will calculate an overly conservative trajectory based on the assumed vehicle length.

## 3. Known issues

### Motion model incompatibility

Since Autoware assumes that vehicles use a steering system, it is not possible to take advantage of the flexibility of a differential drive system's motion model.

For example, when planning a parking maneuver with the `freespace_planner` module, Autoware may drive the differential drive vehicle forward and backward, even if the vehicle can be parked with a simpler trajectory that uses pure rotational movement.
