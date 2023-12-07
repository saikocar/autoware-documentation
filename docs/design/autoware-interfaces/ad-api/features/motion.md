モーション
関連するAPI
{{ link_ad_api('/api/motion/state') }}
{{ link_ad_api('/api/motion/accept_start') }}
説明
この API は車両の現在の動作を管理します。アプリケーションは車両の挙動を周囲の人に通知し、オペレーターや乗客に視覚化することができます。

州
運動状態は車両の停止と発進を管理します。車両が停止すると、状態は STOPPED になります。この後、車両が発進しようとすると（停止中）、状態は STARTING になります。この状態で起動 API を呼び出すと状態が MOVING に変更され、車両が起動します。この仕組みにより、車両発進前のアナウンスなどの処理を追加することができます。構成によっては、状態が STOPPED から MOVING に直接遷移する場合があります。

運動状態

州	説明
停止	車両が停止している。
起動	車両は停止していますが、発進しようとしています。
移動中	車両は動いています。
ブレーキング (未定)	車両が強く減速しています。
# Motion

## Related API

- {{ link_ad_api('/api/motion/state') }}
- {{ link_ad_api('/api/motion/accept_start') }}

## Description

This API manages the current behavior of the vehicle.
Applications can notify the vehicle behavior to the people around and visualize it for operator and passengers.

## States

The motion state manages the stop and start of the vehicle.
Once the vehicle has stopped, the state will be STOPPED.
After this, when the vehicle tries to start (is still stopped), the state will be STARTING.
In this state, calling the start API changes the state to MOVING and the vehicle starts.
This mechanism can add processing such as announcements before the vehicle starts.
Depending on the configuration, the state may transition directly from STOPPED to MOVING.

![motion-state](./motion/state.drawio.svg)

| State            | Description                                     |
| ---------------- | ----------------------------------------------- |
| STOPPED          | The vehicle is stopped.                         |
| STARTING         | The vehicle is stopped, but is trying to start. |
| MOVING           | The vehicle is moving.                          |
| BRAKING (T.B.D.) | The vehicle is decelerating strongly.           |
