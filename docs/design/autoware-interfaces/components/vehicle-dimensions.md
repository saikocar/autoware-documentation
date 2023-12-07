車両寸法
車両の軸とbase_link
車両の軸{: style="幅:500px"}

フレームbase_linkは Autoware スタック全体で非常に頻繁に使用され、リアアクスルの中心を地面に投影したものです。

ローカリゼーション モジュールはmaptobase_link変換を出力します。
base_link計画モジュールは、フレームが将来どこにあるべきかについてポーズを計画します。
制御モジュールは、入力されたポーズに合わせようとしますbase_link。
車両寸法
車両寸法{: style="幅:550px"}

ホイールベース
前車軸と後車軸の間の距離。

トラック幅
左右の車輪間の距離。

オーバーハング
オーバーハングは、セーフティ ボックスの最小計算の一部です。

オーバーハング、サイドミラー、突き出たセンサー、ホイールを測定する場合は考慮する必要があります。

左オーバーハング
左車輪の軸中心と車両の左端の点の間の距離。

右オーバーハング
右車輪の軸中心と車両の右端の点の間の距離。

フロントオーバーハング
フロントアクスルと車両の最前部の間の距離。

リアオーバーハング
後車軸と車両の最後部の間の距離。

車両の長さ
車両の全長。計算方法front_overhang + wheelbase + rear_overhang

車幅_幅
車両の全幅。計算方法left_overhang + track_width + right_overhang

ホイールパラメータ
ホイール寸法{: style="幅:350px"}

ホイール幅
タイヤの横幅。主に推測航法に使用されます。

ホイール半径
ホイールの半径。主に推測航法に使用されます。

ポリゴン_フットプリント
ホイール寸法{: style="幅:350px"}

多角形は、車両の最小衝突エリアを定義します。

点は、 を原点として時計回りに並べる必要がありますbase_link。

ホイールの向き
車両が前進している場合、車輪角度が正の場合、車両は左折します。

Autoware は、後輪がz軸を中心に回転しないことを前提としています。

知らせ
イラストで使用されている車両は xvlblo22 によって作成され、https://www.turbosquid.com/3d-models/modular-sedan-3d-model-1590886からのものです。
# Vehicle dimensions

## Vehicle axes and base_link

![Vehicle Axes](images/vehicle_axes.svg){: style="width:500px"}

The `base_link` frame is used very frequently throughout the Autoware stack, and is a projection of the rear-axle center onto the ground surface.

- Localization module outputs the `map` to `base_link` transformation.
- Planning module plans the poses for where the `base_link` frame should be in the future.
- Control module tries to fit `base_link` to incoming poses.

## Vehicle dimensions

![Vehicle Dimensions](images/vehicle_dimensions.svg){: style="width:550px"}

### wheelbase

The distance between front and rear axles.

### track_width

The distance between left and right wheels.

### Overhangs

Overhangs are part of the minimum safety box calculation.

When measuring overhangs, side mirrors, protruding sensors and wheels should be taken into consideration.

#### left_overhang

The distance between the axis centers of the left wheels and the left-most point of the vehicle.

#### right_overhang

The distance between the axis centers of the right wheels and the right-most point of the vehicle.

#### front_overhang

The distance between the front axle and the foremost point of the vehicle.

#### rear_overhang

The distance between the rear axle and the rear-most point of the vehicle.

### vehicle_length

Total length of the vehicle. Calculated by `front_overhang + wheelbase + rear_overhang`

### vehicle_width

Total width of the vehicle. Calculated by `left_overhang + track_width + right_overhang`

### Wheel parameters

![Wheel Dimensions](images/wheels.svg){: style="width:350px"}

#### wheel_width

The lateral width of a wheel tire, primarily used for dead reckoning.

#### wheel_radius

The radius of the wheel, primarily used for dead reckoning.

### polygon_footprint

![Wheel Dimensions](images/polygon_footprint.svg){: style="width:350px"}

The polygon defines the minimum collision area for the vehicle.

The points should be ordered clockwise, with the origin on the `base_link`.

## Wheel orientations

If the vehicle is going forward, a positive wheel angle will result in the vehicle turning left.

Autoware assumes the rear wheels don't turn on `z` axis.

## Notice

<!-- cspell: ignore xvlblo22 -->

The vehicle used in the illustrations was created by xvlblo22 and is from <https://www.turbosquid.com/3d-models/modular-sedan-3d-model-1590886>.
