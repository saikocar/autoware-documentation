# アッカーマン運動学モデル

Autoware は、アッカーマン運動学モデルに基づいた車両の制御入力をサポートするようになりました。 
このセクションでは、アッカーマン運動学モデルの簡単な概念を紹介し、
Autoware がそれを制御する方法について説明します。
Autoware 制御出力 (/control/command/control_cmd) は、
アッカーマン運動学モデルに従って横方向および縦方向のコマンドを発行することを
覚えておいてください。

- 車両がアッカーマン運動学モデルに適合しない場合は、制御コマンドを変更する必要があります。[別のドキュメントでは、アッカーマン運動学モデルの制御入力を差動駆動モデルに変換する方法の例が示されています。](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/integrating-autoware/creating-vehicle-interface-package/customizing-for-differential-drive-model/)

## ジオメトリ

アッカーマン運動モデルの基本的なスタイルは、前部にアッカーマンリンクを備えた 4 つの車輪があり、
後輪によって駆動されます。
アッカーマン運動学モデルの重要な点は、
すべての車輪の軸が同じ点で交差することであり、
これは、
すべての車輪が、半径は異なるが共通の中心点を持つ円軌道を描くことを意味します
(下の図を参照)。
そのため、
ホイールの滑りを最小限に抑え、
タイヤの摩耗を早めにくいという大きなメリットがあります。

一般に、
アッカーマン運動学モデルは、縦方向の速度 $v$ とステアリング角度 $\phi$ を入力として受け入れます。
Autoware では、$\phi$ を反時計回りに操作すると正の値になります。
したがって、下の図のステアリング角度は実際には負になります。

<figure markdown>
  ![ackermann_link](images/Ackermann_WB.png){ align=center }
  <figcaption>
    アッカーマン運動学モデルの基本スタイル。左の図は車両が直進している場合を示し、右の図は車両が右にステアリングしている場合を示しています。
  </figcaption>
</figure>

### 制御
さらに、Autoware はブレーキ コマンドやライト コマンドなども提供します (車両インターフェイスの設計を参照)。そのため、これらのコマンドを処理できるデバイスがある限り、車両インターフェイス モジュールはこれらのコマンドに適用できるはずです。

Autoware は、さまざまな種類の発行者から`control_cmd`と名前を付けられた ROS 2 トピックを発行します。
`control_cmd`トピックは、次の内容を含む[`AckermannControlCommand`](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/AckermannControlCommand.idl)タイプのメッセージです。

```bash title="AckermannControlCommand"
  builtin_interfaces/Time stamp
  autoware_auto_control_msgs/AckermannLateralCommand lateral
  autoware_auto_control_msgs/LongitudinalCommand longitudinal
```

そこでは,

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

詳細については、 [AckermannLateralCommand.idl](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/AckermannLateralCommand.idl)および[LongitudinalCommand.idl](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_control_msgs/msg/LongitudinalCommand.idl)を参照してください。

車両インターフェースは、車両の制御デバイスを通じてこれらの制御コマンドを実現する必要があります。

さらに、Autoware はブレーキ コマンドやライト コマンドなども提供します([車両インターフェイスの設計](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-interfaces/components/vehicle-interface/)を参照)。そのため、これらのコマンドを処理できるデバイスがある限り、車両インターフェイス モジュールはこれらのコマンドに適用できるはずです。
