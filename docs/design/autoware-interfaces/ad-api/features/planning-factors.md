計画要素
関連するAPI
{{ link_ad_api('/api/planning/velocity_factors') }}
{{ link_ad_api('/api/planning/steering_factors') }}
説明
この API は、車両の計画された動作を管理します。アプリケーションは車両の挙動を周囲の人に通知し、オペレーターや乗客に視覚化することができます。

速度係数
速度係数は、車両が停止または減速する動作に関する一連の情報です。各要因には、以下で説明する動作タイプがあります。一部の動作タイプには、追加情報としてシーケンスと詳細が含まれます。

行動	説明
周囲の障害物	車両のすぐ周囲に障害物があります。
ルート障害物	この先のルートには障害物があります。
交差点	進路上の他の車線に障害物があります。
横断歩道	横断歩道に障害物があります。
後方確認	背後には人間のドライバーの死角となる障害物があります。
ユーザー定義の注目領域	事前に定義された注意エリアに障害物があります。
一時停止禁止区域	停車禁止エリアを超えると十分なスペースがありません。
一時停止標識	一時停止の標識で一時停止。
交通信号	信号で一時停止。
v2x-ゲートエリア	ゲートエリアに立ち寄ります。シーケンスとして Enter と Leave があり、詳細として v2x タイプがあります。
マージ	車線合流前の一時停止。
歩道	歩道を渡る前に一時停止。
車線変更	車線変更です。
回避	現在の車線の障害物を回避するための経路変更。
緊急手術	オペレーターからの緊急指示による停止。
各要素は、ステータス、ベース リンク フレーム内のポーズ、およびそのポーズからの距離も提供します。車両が停止位置に近づくと、この要因は「接近中」というステータスで表示されます。そして、車両がその位置に到達して停止すると、ステータスは STOPPED になります。ポーズは停止位置を示します。停止位置が計算できない場合はベース リンクを示します。

速度係数

ステアリングファクター
ステアリング要素は、左折または右折など、方向指示器の使用を必要とする操縦に関する一連の情報です。各要因には、以下に説明する動作タイプとステアリング方向があります。一部の動作タイプには、追加情報としてシーケンスと詳細が含まれます。

行動	説明
交差点	交差点での左折または右折。
車線変更	車線変更です。
回避	障害物を避けるための経路変更。変化と復帰のシーケンスがあります。
スタートプランナー	未定
目標プランナー	未定
緊急手術	オペレーターの緊急指示による経路変更。
各要素は、ステータス、ベース リンク フレーム内のポーズ、およびそのポーズからの距離も提供します。車両がステアリングを開始する位置に近づくと、この要素は APPROACHING というステータスで表示されます。車両がその位置に到達すると、ステータスは TURNING になります。ポーズは、ステータスが TURNING であるセクションの開始位置と終了位置を示します。

ステアリングファクター-1

車線変更や回避など、状況に応じて範囲内の任意の位置からステアリングを開始します。これらのタイプの場合、ステータスが TURNING であるセクションが動的に更新され、ポーズはそれに追従します。

ステアリングファクター-2
# Planning factors

## Related API

- {{ link_ad_api('/api/planning/velocity_factors') }}
- {{ link_ad_api('/api/planning/steering_factors') }}

## Description

This API manages the planned behavior of the vehicle.
Applications can notify the vehicle behavior to the people around and visualize it for operator and passengers.

## Velocity factors

The velocity factors is an array of information on the behavior that the vehicle stops or slows down.
Each factor has a behavior type which is described below.
Some behavior types have sequence and details as additional information.

| Behavior                    | Description                                                                         |
| --------------------------- | ----------------------------------------------------------------------------------- |
| surrounding-obstacle        | There are obstacles immediately around the vehicle.                                 |
| route-obstacle              | There are obstacles along the route ahead.                                          |
| intersection                | There are obstacles in other lanes in the path.                                     |
| crosswalk                   | There are obstacles on the crosswalk.                                               |
| rear-check                  | There are obstacles behind that would be in a human driver's blind spot.            |
| user-defined-attention-area | There are obstacles in the predefined attention area.                               |
| no-stopping-area            | There is not enough space beyond the no stopping area.                              |
| stop-sign                   | A stop by a stop sign.                                                              |
| traffic-signal              | A stop by a traffic signal.                                                         |
| v2x-gate-area               | A stop by a gate area. It has enter and leave as sequences and v2x type as details. |
| merge                       | A stop before merging lanes.                                                        |
| sidewalk                    | A stop before crossing the sidewalk.                                                |
| lane-change                 | A lane change.                                                                      |
| avoidance                   | A path change to avoid an obstacle in the current lane.                             |
| emergency-operation         | A stop by emergency instruction from the operator.                                  |

Each factor also provides status, poses in the base link frame, and distance from that pose.
As the vehicle approaches the stop position, this factor appears with a status of APPROACHING.
And when the vehicle reaches that position and stops, the status will be STOPPED.
The pose indicates the stop position, or the base link if the stop position cannot be calculated.

![velocity-factors](./planning-factors/velocity-factors.drawio.svg)

## Steering factors

The steering factors is an array of information on the maneuver that requires use of turn indicators, such as turning left or right.
Each factor has a behavior type which is described below and steering direction.
Some behavior types have sequence and details as additional information.

| Behavior            | Description                                                                 |
| ------------------- | --------------------------------------------------------------------------- |
| intersection        | A turning left or right at an intersection.                                 |
| lane-change         | A lane change.                                                              |
| avoidance           | A path change to avoid an obstacle. It has a sequence of change and return. |
| start-planner       | T.B.D.                                                                      |
| goal-planner        | T.B.D.                                                                      |
| emergency-operation | A path change by emergency instruction from the operator.                   |

Each factor also provides status, poses in the base link frame, and distances from that poses.
As the vehicle approaches the position to start steering, this factor appears with a status of APPROACHING.
And when the vehicle reaches that position, the status will be TURNING.
The poses indicate the start and end position of the section where the status is TURNING.

![steering-factors-1](./planning-factors/steering-factors-1.drawio.svg)

In cases such as lane change and avoidance, the vehicle will start steering at any position in the range depending on the situation.
For these types, the section where the status is TURNING will be updated dynamically and the poses will follow that.

![steering-factors-2](./planning-factors/steering-factors-2.drawio.svg)
