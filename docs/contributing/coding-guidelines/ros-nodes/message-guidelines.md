# メッセージのガイドライン

## 形式

すべてのメッセージは[ROSメッセージ記述仕様](https://docs.ros.org/en/humble/Concepts/About-ROS-Interfaces.html#background)に従う必要があります。

受け入れられる形式は以下のとおりです:

- `.msg`
- `.srv`
- `.action`

## 命名法則

!!! 警告 ""

    構築中

複数のタイプのメッセージを作成する場合は`Array`を接尾辞として使用します。この接尾辞は[common_interfaces](https://github.com/ros2/common_interfaces)でよく使用されます。

## 基本単位

デフォルトではすべてのフィールドにはそのタイプに応じて以下の単位があります:

| タイプ           | 基本単位  |
| -------------- | ------------- |
| 距離       | メートル (m)     |
| 角度          | ラジアン (rad) |
| 時間           | 秒 (s)    |
| 速さ          | m/s           |
| 速度       | m/s           |
| 加速度   | m/s²          |
| 角速度   | rad/s         |
| 角加速度 | rad/s²        |

!!! 警告 ""

    メッセージ内のフィールドにこれらのデフォルト単位のいずれかが含まれている場合は、タイプを示すサフィックスやプレフィックスを追加しないでください。

## 非基本単位

デフォルト以外の単位の場合は、次の接尾辞を使用します:

| タイプ     | 非基本単位 | 接尾辞  |
| -------- | ---------------- | ------- |
| 距離 | ナノメートル        | `_nm`   |
| 距離 | マイクロメートル       | `_um`   |
| 距離 | ミリメートル       | `_mm`   |
| 距離 | キロメートル        | `_km`   |
| 角度    | 度 (deg)     | `_deg`  |
| 時間     | ナノ秒       | `_ns`   |
| 時間     | マイクロ秒      | `_us`   |
| 時間     | ミリ秒      | `_ms`   |
| 時間     | 分           | `_min`  |
| 時間     | 時間 (h)         | `_hour` |
| 速度 | km/h             | `_kmph` |

!!! ヒント ""

    使用したいユニットがここに存在しない場合は[問題/PRを作成](https://github.com/autowarefoundation/autoware-documentation/issues)してこのリストに追加します。

## メッセージフィールドのタイプ

ROSインターフェイスでサポートされているタイプのリストについては[ここを参照](https://docs.ros.org/en/humble/Concepts/About-ROS-Interfaces.html#field-types)してください。

便宜のため以下にコピーします:

| メッセージフィールドの型 | C++で等価   |
| ------------------ | ---------------- |
| `bool`             | `bool`           |
| `byte`             | `uint8_t`        |
| `char`             | `char`           |
| `float32`          | `float`          |
| `float64`          | `double`         |
| `int8`             | `int8_t`         |
| `uint8`            | `uint8_t`        |
| `int16`            | `int16_t`        |
| `uint16`           | `uint16_t`       |
| `int32`            | `int32_t`        |
| `uint32`           | `uint32_t`       |
| `int64`            | `int64_t`        |
| `uint64`           | `uint64_t`       |
| `string`           | `std::string`    |
| `wstring`          | `std::u16string` |

### 配列

配列の場合は`無制限の動的配列`タイプを使用します。

例:

```text
int32[] unbounded_integer_array
```

## 列挙

ROS2インターフェイスは列挙を直接サポートしません。

整数定数を定義し、それを非定数整数パラメータに割り当てることができます。

!!! 成功 ""

    定数は`CONSTANT_CASE`に記述します。

!!! 成功 ""

    定数の各要素に異なる値を割り当てます。

[shape_msgs/msg/SolidPrimitive.msg](https://github.com/ros2/common_interfaces/blob/f3cb4848560e91596e7688e8ac1816828fa460cb/shape_msgs/msg/SolidPrimitive.msg#L4-L11)の例

```text
uint8 BOX=1
uint8 SPHERE=2
uint8 CYLINDER=3
uint8 CONE=4
uint8 PRISM=5

# 形状の種類
uint8 type
```

## コメント

メッセージの上にメッセージの内容や用途を簡単に説明します。例については[sensor_msgs/msg/Imu.msg](https://github.com/ros2/common_interfaces/blob/master/sensor_msgs/msg/Imu.msg#L1-L13)を参照してください。

!!! ヒント ""

    必要に応じてフィールドの前にコンテキストや意味を説明する行コメントを追加します。

!!! ヒント ""

    `x, y, z, w`のような単純なフィールドの場合は、コメントを追加する必要がない場合があります。

!!! 成功 ""

    厳密にチェックされているわけではありませんが、1行に100文字を超えないようにしてください。

_例:_

```text
# 車両が緊急ブレーキを実行した回数
uint32 count_emergency_brake

# 最後の緊急ブレーキから経過した秒数
uint64 duration
```

## 使用例

- デフォルトのタイプには単位接尾辞を使用しないでください:
  - 悪い: `float32 path_length_m`
  - 良い: `float32 path_length`
- 単位を接頭辞にしないでください:
  - 悪い: `float32 kmph_velocity_vehicle`
  - 良い: `float32 velocity_vehicle_kmph`
- 推奨される接尾辞が[表にある場合](#non-default-units)はそれを使用します:
  - 悪い: `float32 velocity_vehicle_km_h`
  - 良い: `float32 velocity_vehicle_kmph`
