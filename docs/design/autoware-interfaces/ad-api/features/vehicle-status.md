車両状態
関連するAPI
{{ link_ad_api('/api/vehicle/kinematics') }}
{{ link_ad_api('/api/vehicle/status') }}
{{ link_ad_api('/api/vehicle/dimensions') }}
運動学
これは車両の運動学の推定値です。車両位置は配車スケジュールの申請に必要です。また、アプリケーションは速度と加速度を使用して、立ち往生や急ブレーキなど、オペレーターの支援が必要な車両を見つけることができます。

状態
これは車両が提供するステータスです。インジケーターとステアリングは主に視覚化と遠隔制御に使用されます。残りのエネルギーは車両のスケジュール設定にも使用できます。

寸法
運動学における車両の位置はベース リンクの座標であるため、車両の寸法は車両とオブジェクト間の実際の距離を知るために使用されます。これは車両を遠隔からサポートする際の視覚化に必要です。
# Vehicle status

## Related API

- {{ link_ad_api('/api/vehicle/kinematics') }}
- {{ link_ad_api('/api/vehicle/status') }}
- {{ link_ad_api('/api/vehicle/dimensions') }}

## Kinematics

This is an estimate of the vehicle kinematics. The vehicle position is necessary for applications to schedule dispatches.
Also, using velocity and acceleration, applications can find vehicles that need operator assistance, such as stuck or brake suddenly.

## Status

This is the status provided by the vehicle. The indicators and steering are mainly used for visualization and remote control.
The remaining energy can be also used for vehicle scheduling.

## Dimensions

The vehicle dimensions are used to know the actual distance between the vehicle and objects because the vehicle position in kinematics is the coordinates of the base link. This is necessary for visualization when supporting vehicles remotely.
