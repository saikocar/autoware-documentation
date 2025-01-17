感知

入力
ポイントクラウド
Lidar によって公開された PointCloud データ。

センサー_msgs/msg/PointCloud2
画像
カメラで捉えた画像フレーム。

センサー_msgs/msg/画像
車両の運動学的状態
信号認識に使用される自我の現在位置。Localization の出力を参照してください。

レーンレット2の地図
環境の地図。Map の出力を参照してください。

出力
3D オブジェクトの予測
3D オブジェクトはセンサー フュージングによって検出、追跡、予測されます。

autoware_auto_perception_msgs/msg/PredictedObjects
std_msgs/Headerヘッダー
sequence< autoware_auto_perception_msgs::msg::PredictedObject > オブジェクト
unique_identifier_msgs::msg::UUID uuid
浮動小数点数の存在確率
シーケンス< autoware_auto_perception_msgs::msg::ObjectClassification > 分類
uint8 分類
浮動確率
autoware_auto_perception_msgs::msg::PredictedObjectKinematicsキネマティクス
geometry_msgs::msg:: PoseWithCovarianceinitial_pose
geometry_msgs::msg::TwistWithCovariance
geometry_msgs::msg:: AccelWithCovarianceinitial_acceleration
シーケンス< autoware_auto_perception_msgs::msg::PredictedPath , 10> 予測パス
シーケンス< geometry_msgs::msg::Pose , 100> パス
builtin_interfaces::msg::Duration time_step
フロート自信
sequence< autoware_auto_perception_msgs::msg::Shape , 5> 形状
geometry_msgs::msg::Polygonポリゴン
フロートの高さ
信号機
物体検出モデルにより認識される信号機。

autoware_perception_msgs::msg::TrafficSignalArray
autoware_perception_msgs::msg::TrafficSignal信号
autoware_perception_msgs::msg::TrafficSignalElement要素
uint8 不明 = 0
uint8 赤 = 1
uint8 アンバー = 2
uint8 ホワイト = 4
uint8 サークル = 1
uint8 LEFT_ARROW = 2
uint8 右矢印 = 3
uint8 UP_ARROW = 4
uint8 UP_LEFT_ARROW=5
uint8 UP_RIGHT_ARROW=6
uint8 下矢印 = 7
uint8 下向き矢印 = 8
uint8 下向き矢印 = 9
uint8 クロス = 10
uint8 SOLID_OFF = 1
uint8 SOLID_ON = 2
uint8 フラッシュ = 3
uint8 色
uint8 形状
uint8 ステータス
float32 の信頼度
int64 トラフィック信号 ID
builtin_interfaces::msg::タイムスタンプ
# Perception

```mermaid
graph TD
    cmp_sen("Sensing"):::cls_sen
    cmp_loc("Localization"):::cls_loc
    cmp_per("Perception"):::cls_per
    cmp_plan("Planning"):::cls_plan

    msg_img("<font size=2><b>Camera Image</b></font size>
    <font size=1>sensor_msgs/Image</font size>"):::cls_sen

    msg_ldr("<font size=2><b>Lidar Point Cloud</b></font size>
    <font size=1>sensor_msgs/PointCloud2</font size>"):::cls_sen

    msg_lanenet("<font size=2><b>Lanelet2 Map</b></font size>
    <font size=1>autoware_auto_mapping_msgs/HADMapBin</font size>"):::cls_loc

    msg_vks("<font size=2><b>Vehicle Kinematic State</b></font size>
    <font size=1>nav_msgs/Odometry</font size>"):::cls_loc

    msg_obj("<font size=2><b>3D Object Predictions </b></font size>
    <font size=1>autoware_auto_perception_msgs/PredictedObjects</font size>"):::cls_per

    msg_tl("<font size=2><b>Traffic Light Response </b></font size>
    <font size=1>autoware_perception_msgs/TrafficSignalArray</font size>"):::cls_per

    msg_tq("<font size=2><b>Traffic Light Query </b></font size>
    <font size=1>TBD</font size>"):::cls_plan


    cmp_sen --> msg_img --> cmp_per
    cmp_sen --> msg_ldr --> cmp_per
    cmp_per --> msg_obj --> cmp_plan
    cmp_per --> msg_tl --> cmp_plan
    cmp_plan --> msg_tq -->cmp_per

    cmp_loc --> msg_vks --> cmp_per
    cmp_loc --> msg_lanenet --> cmp_per

classDef cmp_sen fill:#F8CECC,stroke:#999,stroke-width:1px;
classDef cls_loc fill:#D5E8D4,stroke:#999,stroke-width:1px;
classDef cls_per fill:#FFF2CC,stroke:#999,stroke-width:1px;
classDef cls_plan fill:#5AB8FF,stroke:#999,stroke-width:1px;
```

## Inputs

### PointCloud

PointCloud data published by Lidar.

- [sensor_msgs/msg/PointCloud2](http://docs.ros.org/en/api/sensor_msgs/html/msg/PointCloud2.html)

### Image

Image frame captured by camera.

- [sensor_msgs/msg/Image](http://docs.ros.org/en/api/sensor_msgs/html/msg/Image.html)

### Vehicle kinematic state

current position of ego, used in traffic signals recognition. See [output of Localization](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-interfaces/components/localization/#vehicle-kinematic-state).

### Lanelet2 Map

map of the environment. See [outputs of Map](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-interfaces/components/map/#outputs).

## Output

### 3D Object Predictions

3D Objects detected, tracked and predicted by sensor fusing.

- [autoware_auto_perception_msgs/msg/PredictedObjects](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/PredictedObjects.idl)
  - [std_msgs/Header](https://docs.ros.org/en/noetic/api/std_msgs/html/msg/Header.html) header
  - sequence<[autoware_auto_perception_msgs::msg::PredictedObject](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/PredictedObject.idl)> objects
    - unique_identifier_msgs::msg::UUID uuid
    - float existence_probability
    - sequence<[autoware_auto_perception_msgs::msg::ObjectClassification](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/ObjectClassification.idl)> classification
      - uint8 classification
      - float probability
    - [autoware_auto_perception_msgs::msg::PredictedObjectKinematics](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/PredictedObjectKinematics.idl) kinematics
      - [geometry_msgs::msg::PoseWithCovariance](https://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/PoseWithCovariance.html) initial_pose
      - [geometry_msgs::msg::TwistWithCovariance](https://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/TwistWithCovariance.html)
      - [geometry_msgs::msg::AccelWithCovariance](https://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/AccelWithCovariance.html) initial_acceleration
      - sequence<[autoware_auto_perception_msgs::msg::PredictedPath](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/PredictedPath.idl), 10> predicted_paths
        - sequence<[geometry_msgs::msg::Pose](https://docs.ros.org/en/lunar/api/geometry_msgs/html/msg/Pose.html), 100> path
        - builtin_interfaces::msg::Duration time_step
        - float confidence
    - sequence<[autoware_auto_perception_msgs::msg::Shape](https://gitlab.com/autowarefoundation/autoware.auto/autoware_auto_msgs/-/blob/master/autoware_auto_perception_msgs/msg/Shape.idl), 5> shape
      - [geometry_msgs::msg::Polygon](https://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/Polygon.html) polygon
      - float height

### Traffic Signals

traffic signals recognized by object detection model.

- [autoware_perception_msgs::msg::TrafficSignalArray](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_perception_msgs/msg/TrafficSignalArray.msg)
  - [autoware_perception_msgs::msg::TrafficSignal](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_perception_msgs/msg/TrafficSignal.msg) signals
    - [autoware_perception_msgs::msg::TrafficSignalElement](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_perception_msgs/msg/TrafficSignalElement.msg) elements
      - uint8 UNKNOWN = 0
      - uint8 Red = 1
      - uint8 AMBER = 2
      - uint8 WHITE = 4
      - uint8 CIRCLE = 1
      - uint8 LEFT_ARROW = 2
      - uint8 RIGHT_ARROW = 3
      - uint8 UP_ARROW = 4
      - uint8 UP_LEFT_ARROW=5
      - uint8 UP_RIGHT_ARROW=6
      - uint8 DOWN_ARROW = 7
      - uint8 DOWN_LEFT_ARROW = 8
      - uint8 DOWN_RIGHT_ARROW = 9
      - uint8 CROSS = 10
      - uint8 SOLID_OFF = 1
      - uint8 SOLID_ON = 2
      - uint8 FLASHING = 3
      - uint8 color
      - uint8 shape
      - uint8 status
      - float32 confidence
    - int64 traffic_signal_id
  - [builtin_interfaces::msg::Time](https://github.com/ros2/rcl_interfaces/blob/rolling/builtin_interfaces/msg/Time.msg) stamp
