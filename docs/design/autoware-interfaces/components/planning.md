企画
このページでは、計画コンポーネントのインターフェイスに関する具体的な仕様を提供します。高レベルの概念とデータ フローについては、計画アーキテクチャ設計ドキュメントを参照してください。

TODO: 詳細な定義 (各トピックに含まれる要素の意味) がまだ記述されていないため、更新する必要があります。

入力
計画入力

マップコンポーネントから
名前	トピック	タイプ	説明
ベクトルマップ	/map/vector_map	autoware_auto_mapping_msgs/msg/HADMapBin	計画が行われる環境の地図。
ローカリゼーションコンポーネントから
名前	トピック	タイプ	説明
車両の運動学的状態	/localization/kinematic_state	nav_msgs/msg/オドメトリ	自我の現在の位置、向き、速度。
車両の加速	/localization/acceleration	geometry_msgs/msg/AccelWithCovarianceStamped	現在のエゴの加速。
TODO : 加速度情報を運動学的状態にマージする必要があります。

知覚コンポーネントから
名前	トピック	タイプ	説明
オブジェクト	/perception/object_recognition/objects	autoware_auto_perception_msgs/msg/PredictedObjects	軌道を計画するときに回避または追跡する必要がある、自我の周囲にある知覚されたオブジェクトのセット。これには、オブジェクト クラス (車両、歩行者など) やオブジェクトの形状などのセマンティクス情報が含まれます。
障害物	/perception/obstacle_segmentation/pointcloud	センサー_msgs/msg/PointCloud2	軌道を計画するときに回避または従う必要がある、自我の周囲に認識される一連の障害物。これには障害物のプリミティブな情報のみが含まれます。形状や速度の情報はありません。
占有グリッドマップ	/perception/occupancy_grid_map/map	nav_msgs/msg/OccupancyGrid	障害物の存在と死角情報が含まれます (UNKNOWN として表されます)。
信号機	/perception/traffic_light_recognition/traffic_signals	autoware_auto_perception_msgs/msg/TrafficSignalArray	信号の色（緑、黄、読み取り）や矢印（右、左、直進）などの信号情報が含まれています。
TODO : 情報のタイプは、Obstacles特定のセンサー メッセージ タイプに依存すべきではありません (現時点ではPointCloud)。修正する必要があります。

APIから
名前	トピック	タイプ	説明
最大速度	/planning/scenario_planning/max_velocity_default	autoware_adapi_v1_msgs/srv/SetRoutePoints	車速プランの最大値を示す
動作モード	/system/operation_mode/state	autoware_adapi_v1_msgs/msg/OperationModeState	現在の動作モード（自動/手動など）を示します。
ルートセット	/planning/mission_planning/set_route	autoware_adapi_v1_msgs/srv/SetRoute	停車時にルートを設定することを示します。
ルート ポイント セット	/planning/mission_planning/set_route_points	autoware_adapi_v1_msgs/srv/SetRoutePoints	停車時にポイントでルートを設定することを示します。
ルート変更	/planning/mission_planning/change_route	autoware_adapi_v1_msgs/srv/SetRoute	車両の走行中にルートを変更することを示します。
ルートポイントの変更	/planning/mission_planning/change_route_points	autoware_adapi_v1_msgs/srv/SetRoutePoints	車両の走行中にポイントでルートを変更することを示します。
ルートクリア	/planning/mission_planning/clear_route	autoware_adapi_v1_msgs/srv/ClearRoute	ルート情報をクリアすることを示します。
MRM ルート設定ポイント	/planning/mission_planning/mission_planner/srv/set_mrm_route	autoware_adapi_v1_msgs/srv/SetRoutePoints	緊急ルートを設定することを示します。
MRMルートクリア	/planning/mission_planning/mission_planner/srv/clear_mrm_route	autoware_adapi_v1_msgs/srv/SetRoutePoints	緊急ルートをクリアすることを示します。
出力
計画出力

制御するには
名前	トピック	タイプ	説明
軌跡	/planning/trajectory	autoware_auto_planning_msgs/msg/Trajectory	コントローラーが従う空間、速度、加速度のポイントのシーケンス。
方向指示器	/planning/turn_indicators_cmd	autoware_auto_vehicle_msgs/msg/TurnIndicatorsコマンド	車両が従う方向指示器信号。
ハザードランプ	/planning/hazard_lights_cmd	autoware_auto_vehicle_msgs/msg/HazardLightsCommand	車両が従うべきハザード信号。
システムへ
名前	トピック	タイプ	説明
診断	/planning/hazard_lights_cmd	Diagnostic_msgs/msg/DiagnosticArray	計画コンポーネントの診断ステータスがシステム コンポーネントに報告されます。
APIへ
名前	トピック	タイプ	説明
パス候補	/planning/path_candidate/*	autoware_auto_planning_msgs/msg/パス	Autoware が取ろうとしている道。ユーザは、経路候補情報に基づいて操作を中断することができます。
ステアリングファクター	/planning/steering_factor/*	autoware_adapi_v1_msgs/msg/SteeringFactorArray	Autoware によって実行されるステアリング操作に関する情報 (例: 右折のための右へのステアリングなど)
速度係数	/planning/velocity_factors/*	autoware_adapi_v1_msgs/msg/VelocityFactorArray	Autoware によって実行される速度操作に関する情報 (障害物による停止など)
内部インターフェースの計画
このセクションでは、計画アーキテクチャ設計に示されているさまざまな計画モジュール間の通信について説明します。

企画内部

ミッションプランニングからシナリオプランニングまで
名前	トピック	タイプ	説明
ルート	/planning/mission_planning/route	autoware_planning_msgs/msg/LaneletRoute	Lanelet マップ上の出発地から目的地までのレーン ID のシーケンス。
行動計画から動作計画へ
名前	トピック	タイプ	説明
パス	/planning/scenario_planning/lane_driving/behavior_planning/path	autoware_auto_planning_msgs/msg/パス	運転のための一連のおおよその車両位置と、最高速度および走行可能エリアに関する情報。このメッセージを受信したモジュールは、走行可能領域と最大速度の制約内で経路を変更し、目的の最終軌道を生成することが期待されます。
シナリオの計画から検証まで
名前	トピック	タイプ	説明
軌跡	/planning/scenario_planning/trajectory	autoware_auto_planning_msgs/msg/Trajectory	運転に必要な一連の正確な車両位置、速度、加速度。車両はこの軌道をたどると予想されます。
# Planning

This page provides specific specifications about the Interface of the Planning Component. Please refer to the [planning architecture design document](../../autoware-architecture/planning/index.md) for high-level concepts and data flow.

**TODO: The detailed definitions (meanings of elements included in each topic) are not described yet, need to be updated.**

## Input

![planning-input](images/planning-interface-input.drawio.svg)

### From Map Component

| Name       | Topic             | Type                                                                                                                                                 | Description                                            |
| ---------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| Vector Map | `/map/vector_map` | [autoware_auto_mapping_msgs/msg/HADMapBin](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_mapping_msgs/msg/HADMapBin.idl) | Map of the environment where the planning takes place. |

### From Localization Component

| Name                    | Topic                           | Type                                                                                                                                      | Description                                        |
| ----------------------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| Vehicle Kinematic State | `/localization/kinematic_state` | [nav_msgs/msg/Odometry](https://docs.ros.org/en/latest/api/nav_msgs/html/msg/Odometry.html)                                               | Current position, orientation and velocity of ego. |
| Vehicle Acceleration    | `/localization/acceleration`    | [geometry_msgs/msg/AccelWithCovarianceStamped](https://docs.ros.org/en/latest/api/geometry_msgs/html/msg/AccelWithCovarianceStamped.html) | Current acceleration of ego.                       |

**TODO**: acceleration information should be merged into the kinematic state.

### From Perception Component

| Name               | Topic                                                   | Type                                                                                                                                                                         | Description                                                                                                                                                                                                               |
| ------------------ | ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Objects            | `/perception/object_recognition/objects`                | [autoware_auto_perception_msgs/msg/PredictedObjects](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_perception_msgs/msg/PredictedObjects.idl)     | Set of perceived objects around ego that need to be avoided or followed when planning a trajectory. This contains semantics information such as a object class (e.g. vehicle, pedestrian, etc) or a shape of the objects. |
| Obstacles          | `/perception/obstacle_segmentation/pointcloud`          | [sensor_msgs/msg/PointCloud2](https://docs.ros.org/en/latest/api/sensor_msgs/html/msg/PointCloud2.html)                                                                      | Set of perceived obstacles around ego that need to be avoided or followed when planning a trajectory. This only contains a primitive information of the obstacle. No shape nor velocity information.                      |
| Occupancy Grid Map | `/perception/occupancy_grid_map/map`                    | [nav_msgs/msg/OccupancyGrid](https://docs.ros.org/en/latest/api/nav_msgs/html/msg/OccupancyGrid.html)                                                                        | Contains the presence of obstacles and blind spot information (represented as UNKNOWN).                                                                                                                                   |
| Traffic Signal     | `/perception/traffic_light_recognition/traffic_signals` | [autoware_auto_perception_msgs/msg/TrafficSignalArray](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_perception_msgs/msg/TrafficSignalArray.idl) | Contains the traffic signal information such as a color (green, yellow, read) and an arrow (right, left, straight).                                                                                                       |

**TODO**: The type of the `Obstacles` information should not depend on the specific sensor message type (now `PointCloud`). It needs to be fixed.

### From API

| Name                 | Topic                                                            | Type                                                                                                                                                                                  | Description                                                           |
| -------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Max Velocity         | `/planning/scenario_planning/max_velocity_default`               | [autoware_adapi_v1_msgs/srv/SetRoutePoints](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoutePoints.srv)                | Indicate the maximum value of the vehicle speed plan                  |
| Operation Mode       | `/system/operation_mode/state`                                   | [autoware_adapi_v1_msgs/msg/OperationModeState](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/operation_mode/msg/OperationModeState.msg) | Indicates the current operation mode (automatic/manual, etc.).        |
| Route Set            | `/planning/mission_planning/set_route`                           | [autoware_adapi_v1_msgs/srv/SetRoute](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoute.srv)                            | Indicates to set the route when the vehicle is stopped.               |
| Route Points Set     | `/planning/mission_planning/set_route_points`                    | [autoware_adapi_v1_msgs/srv/SetRoutePoints](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoutePoints.srv)                | Indicates to set the route with points when the vehicle is stopped.   |
| Route Change         | `/planning/mission_planning/change_route`                        | [autoware_adapi_v1_msgs/srv/SetRoute](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoute.srv)                            | Indicates to change the route when the vehicle is moving.             |
| Route Points Change  | `/planning/mission_planning/change_route_points`                 | [autoware_adapi_v1_msgs/srv/SetRoutePoints](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoutePoints.srv)                | Indicates to change the route with points when the vehicle is moving. |
| Route Clear          | `/planning/mission_planning/clear_route`                         | [autoware_adapi_v1_msgs/srv/ClearRoute](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/ClearRoute.srv)                        | Indicates to clear the route information.                             |
| MRM Route Set Points | `/planning/mission_planning/mission_planner/srv/set_mrm_route`   | [autoware_adapi_v1_msgs/srv/SetRoutePoints](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoutePoints.srv)                | Indicates to set the emergency route.                                 |
| MRM Route Clear      | `/planning/mission_planning/mission_planner/srv/clear_mrm_route` | [autoware_adapi_v1_msgs/srv/SetRoutePoints](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/routing/srv/SetRoutePoints.srv)                | Indicates to clear the emergency route.                               |

## Output

![planning-output](images/planning-interface-output.drawio.svg)

### To Control

| Name           | Topic                           | Type                                                                                                                                                                         | Description                                                                                |
| -------------- | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Trajectory     | `/planning/trajectory`          | [autoware_auto_planning_msgs/msg/Trajectory](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_planning_msgs/msg/Trajectory.idl)                     | A sequence of space and velocity and acceleration points to be followed by the controller. |
| Turn Indicator | `/planning/turn_indicators_cmd` | [autoware_auto_vehicle_msgs/msg/TurnIndicatorsCommand](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/TurnIndicatorsCommand.idl) | Turn indicator signal to be followed by the vehicle.                                       |
| Hazard Light   | `/planning/hazard_lights_cmd`   | [autoware_auto_vehicle_msgs/msg/HazardLightsCommand](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_vehicle_msgs/msg/HazardLightsCommand.idl)     | Hazard light signal to be followed by the vehicle.                                         |

### To System

| Name        | Topic                         | Type                                                                                                                   | Description                                                                   |
| ----------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Diagnostics | `/planning/hazard_lights_cmd` | [diagnostic_msgs/msg/DiagnosticArray](http://docs.ros.org/en/latest/api/diagnostic_msgs/html/msg/DiagnosticArray.html) | Diagnostic status of the Planning component reported to the System component. |

### To API

| Name            | Topic                          | Type                                                                                                                                                                              | Description                                                                                                         |
| --------------- | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Path Candidate  | `/planning/path_candidate/*`   | [autoware_auto_planning_msgs/msg/Path](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_planning_msgs/msg/Path.idl)                                      | The path Autoware is about to take. Users can interrupt the operation based on the path candidate information.      |
| Steering Factor | `/planning/steering_factor/*`  | [autoware_adapi_v1_msgs/msg/SteeringFactorArray](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/planning/msg/SteeringFactorArray.msg) | Information about the steering maneuvers performed by Autoware (e.g., steering to the right for a right turn, etc.) |
| Velocity Factor | `/planning/velocity_factors/*` | [autoware_adapi_v1_msgs/msg/VelocityFactorArray](https://github.com/autowarefoundation/autoware_adapi_msgs/blob/main/autoware_adapi_v1_msgs/planning/msg/VelocityFactorArray.msg) | Information about the velocity maneuvers performed by Autoware (e.g., stop for an obstacle, etc.)                   |

## Planning internal interface

This section explains the communication between the different planning modules shown in the [Planning Architecture Design](../../autoware-architecture/planning/index.md#high-level-architecture).

![planning-internal](images/planning-interface-internal.drawio.svg)

### From Mission Planning to Scenario Planning

| Name  | Topic                              | Type                                                                                                                                                 | Description                                                                          |
| ----- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Route | `/planning/mission_planning/route` | [autoware_planning_msgs/msg/LaneletRoute](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_planning_msgs/msg/LaneletRoute.msg) | A sequence of lane IDs on a Lanelet map, from the starting point to the destination. |

### From Behavior Planning to Motion Planning

| Name | Topic                                                             | Type                                                                                                                                         | Description                                                                                                                                                                                                                                                                                                       |
| ---- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Path | `/planning/scenario_planning/lane_driving/behavior_planning/path` | [autoware_auto_planning_msgs/msg/Path](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_planning_msgs/msg/Path.idl) | A sequence of approximate vehicle positions for driving, along with information on the maximum speed and the drivable areas. Modules receiving this message are expected to make changes to the path within the constraints of the drivable areas and the maximum speed, generating the desired final trajectory. |

### From Scenario Planning to Validation

| Name       | Topic                                    | Type                                                                                                                                                     | Description                                                                                                                                           |
| ---------- | ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trajectory | `/planning/scenario_planning/trajectory` | [autoware_auto_planning_msgs/msg/Trajectory](https://github.com/tier4/autoware_auto_msgs/blob/tier4/main/autoware_auto_planning_msgs/msg/Trajectory.idl) | A sequence of precise vehicle positions, speeds, and accelerations required for driving. It is expected that the vehicle will follow this trajectory. |
