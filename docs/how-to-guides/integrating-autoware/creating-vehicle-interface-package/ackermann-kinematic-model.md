アッカーマン運動学モデル
Autoware は、アッカーマン運動学モデルに基づいた車両の制御入力をサポートするようになりました。
このセクションでは、アッカーマン運動学モデルの簡単な概念を紹介し、Autoware がそれを制御する方法について説明します。Autoware 制御出力 (/control/command/control_cmd) は、アッカーマン運動学モデルに従って横方向および縦方向のコマンドを発行することを覚えておいてください。

車両がアッカーマン運動学モデルに適合しない場合は、制御コマンドを変更する必要があります。別のドキュメントでは、アッカーマン運動学モデルの制御入力を差動駆動モデルに変換する方法の例が示されています。
ジオメトリ
アッカーマン運動モデルの基本的なスタイルは、前部にアッカーマンリンクを備えた 4 つの車輪があり、後輪によって駆動されます。
アッカーマン運動学モデルの重要な点は、すべての車輪の軸が同じ点で交差することです。これは、すべての車輪が、半径は異なるが共通の中心点を持つ円軌道を描くことを意味します (下の図を参照)。
そのため、ホイールの滑りを最小限に抑え、タイヤの摩耗を早めにくいという大きなメリットがあります。

一般に、アッカーマン運動学モデルは縦方向の速度を受け入れます。
そしてステアリング角度
入力として。オートウェアでは、
反時計回りに操舵すると角度は正になるため、下図の操舵角は実際には負になります。

![ackermann_link](images/Ackermann_WB.png){ align=center } アッカーマン運動学モデルの基本スタイル。左の図は車両が直進している場合を示し、右の図は車両が右にステアリングしている場合を示しています。
コントロール
Autoware は、さまざまな種類の発行者から名前を付けた ROS 2 トピックを発行しますcontrol_cmd。
トピックは、次の内容を含むタイプのメッセージcontrol_cmdです。AckermannControlCommand

  builtin_interfaces/Time stamp
  autoware_auto_control_msgs/AckermannLateralCommand lateral
  autoware_auto_control_msgs/LongitudinalCommand longitudinal
どこ、

  builtin_interfaces/Time stamp
  float32 steering_tire_angle
  float32 steering_tire_rotation_rate
  builtin_interfaces/Time stamp
  float32 speed
  float32 accelaration
  float32 jerk
詳細については、 AckermannLateralCommand.idlおよびLongitudinalCommand.idlを参照してください。

車両インターフェースは、車両の制御デバイスを通じてこれらの制御コマンドを実現する必要があります。

さらに、Autoware はブレーキ コマンドやライト コマンドなども提供します (車両インターフェイスの設計を参照)。そのため、これらのコマンドを処理できるデバイスがある限り、車両インターフェイス モジュールはこれらのコマンドに適用できるはずです。
# Ackermann kinematic model

Autoware now supports control inputs for vehicles based on an Ackermann kinematic model.  
This section introduces you a brief concept of the Ackermann kinematic model
and explains how Autoware controls it.
Please remember,
Autoware control output (/control/command/control_cmd)
publishes lateral and longitudinal commands according to the Ackermann kinematic model.

- If your vehicle does not suit the Ackermann kinematic model, you have to modify the control commands. [Another document gives you an example how to convert your Ackermann kinematic model control inputs into a differential drive model.](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/integrating-autoware/creating-vehicle-interface-package/customizing-for-differential-drive-model/)

## Geometry

The basic style of the Ackermann kinematic model has four wheels with an Ackermann link on the front,
and it is powered by the rear wheels.  
The key point of Ackermann kinematic model is
that the axes of all wheels intersect at the same point,
which means
all wheels will trace a circular trajectory with a different radii but a common center point
(See the figure below).  
Therefore,
this model has a great advantage that it minimizes the slippage of the wheels
and prevents tires from getting worn soon.

In general,
the Ackermann kinematic model accepts the longitudinal speed $v$ and the steering angle $\phi$ as inputs.
In autoware, $\phi$ is positive if it is steered counterclockwise,
so the steering angle in the figure below is actually negative.

<figure markdown>
  ![ackermann_link](images/Ackermann_WB.png){ align=center }
  <figcaption>
    The basic style of an Ackermann kinematic model. The left figure shows a vehicle facing straight forward, while the right figure shows a vehicle steering to the right.
  </figcaption>
</figure>

### Control

Autoware publishes a ROS 2 topic named `control_cmd` from several types of publishers.  
A `control_cmd` topic is a [`AckermannControlCommand`](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/AckermannControlCommand.idl) type message that contains

```bash title="AckermannControlCommand"
  builtin_interfaces/Time stamp
  autoware_auto_control_msgs/AckermannLateralCommand lateral
  autoware_auto_control_msgs/LongitudinalCommand longitudinal
```

where,

```bash title="AckermannLateralCommand"
  builtin_interfaces/Time stamp
  float32 steering_tire_angle
  float32 steering_tire_rotation_rate
```

```bash title="LongitudinalCommand"
  builtin_interfaces/Time stamp
  float32 speed
  float32 accelaration
  float32 jerk
```

See the [AckermannLateralCommand.idl](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/AckermannLateralCommand.idl) and [LongitudinalCommand.idl](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/LongitudinalCommand.idl) for details.

The vehicle interface should realize these control commands through your vehicle's control device.

Moreover, Autoware also provides brake commands, light commands, and more (see [vehicle interface design](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-interfaces/components/vehicle-interface/)), so the vehicle interface module should be applicable to these commands as long as there are devices available to handle them.
