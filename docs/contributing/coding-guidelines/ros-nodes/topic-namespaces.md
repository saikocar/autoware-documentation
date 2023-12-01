# トピックの名前空間

## 概要

ROS では、トピック、パラメータ、ノードに名前空間を付けることができ、以下のような利点があります:

- 同じノード タイプの複数のインスタンスによって名前の衝突が発生することはありません。
- ノードによって公開されたトピックには、ノードの名前空間を使用して自動的に名前空間が設定され、意味のあるわかりやすい接続が提供されます。
- 根元の名前空間が乱雑になるのを防ぎます。
- 関心事の分離を維持するのに役立ちます。

このページでは、Autoware での名前空間の使用方法に焦点を当て、いくつかの役立つ例を示します。トピック名前空間の基本情報については、[このチュートリアル](https://design.ros2.org/articles/topic_and_service_names.html)を参照してください。

## ノード内でトピックに名前を付ける方法

Autoware はノードを次の機能カテゴリに分割し、カテゴリに従ってノードの開始名前空間を追加します。

- 位置推定
- 知覚
- 計画
- 操作
- 計測
- 車両
- 地図
- システム

ノードが名前空間で実行されると、そのノードが公開するすべてのトピックに同じ名前空間が与えられます。 Autoware スタック内のすべてのノードは、グローバル名前空間でトピックを公開するなどの行為を避けて、名前空間をサポートする必要があります。

一般に、トピックは、トピックを消費するノードではなく、トピックを生成するノードの機能に基づいて名前空間を設定する必要があります。

ノードによってサブスクライブまたはパブリッシュされているかに基づいて、トピックを入力トピックまたは出力トピックとして分類します。ノードでは、入力トピックの名前は`input/topic_name`、出力トピックの名前は`output/topic_name`です。

ノードの起動ファイルでトピックを構成します。`joy_controller`ノードを例に挙げます。以下の例では`joy_controller.launch.xml`ファイルで入力トピックと出力トピックを設定し、トピックを再割り当てします。

```xml
<launch>
  <arg name="input_joy" default="/joy"/>
  <arg name="input_odometry" default="/localization/kinematic_state"/>

  <arg name="output_control_command" default="/external/$(var external_cmd_source)/joy/control_cmd"/>
  <arg name="output_external_control_command" default="/api/external/set/command/$(var external_cmd_source)/control"/>
  <arg name="output_shift" default="/api/external/set/command/$(var external_cmd_source)/shift"/>
  <arg name="output_turn_signal" default="/api/external/set/command/$(var external_cmd_source)/turn_signal"/>
  <arg name="output_heartbeat" default="/api/external/set/command/$(var external_cmd_source)/heartbeat"/>
  <arg name="output_gate_mode" default="/control/gate_mode_cmd"/>
  <arg name="output_vehicle_engage" default="/vehicle/engage"/>

  <node pkg="joy_controller" exec="joy_controller" name="joy_controller" output="screen">
    <remap from="input/joy" to="$(var input_joy)"/>
    <remap from="input/odometry" to="$(var input_odometry)"/>

    <remap from="output/control_command" to="$(var output_control_command)"/>
    <remap from="output/external_control_command" to="$(var output_external_control_command)"/>
    <remap from="output/shift" to="$(var output_shift)"/>
    <remap from="output/turn_signal" to="$(var output_turn_signal)"/>
    <remap from="output/gate_mode" to="$(var output_gate_mode)"/>
    <remap from="output/heartbeat" to="$(var output_heartbeat)"/>
    <remap from="output/vehicle_engage" to="$(var output_vehicle_engage)"/>
  </node>
</launch>
```

## コード内のトピック名

1. 起動設定の名前空間が適用されるように`~`を付けます (根元`/`から開始しないでください)。

2. 他のノードとの通信に使用されるトピック名の前に`~/input`あるいは`~/output`と名前空間を付けます。

   たとえば、`obstacle_avoidance_planner`は、`~/input/topic_name`のタイプのトピック名を使用してトピックを公開します。

   ```cpp
   objects_sub_ = create_subscription<PredictedObjects>(
    "~/input/objects", rclcpp::QoS{10},
    std::bind(&ObstacleAvoidancePlanner::onObjects, this, std::placeholders::_1));
   ```

   たとえば、`obstacle_avoidance_planner`では、`~/output/topic_name`のタイプのトピック名を使用してトピックを公開します。

   ```cpp
   traj_pub_ = create_publisher<Trajectory>("~/output/path", 1);
   ```

3. 視覚化またはデバッグ目的のトピックには`~/debug/`名前空間が必要です。

   たとえば、`obstacle_avoidance_planner`ノードでは、トピックをデバッグまたは視覚化するために、`~/debug/topic_name`タイプのトピック名を使用して情報を公開します。

   ```cpp
   debug_markers_pub_ =
    create_publisher<visualization_msgs::msg::MarkerArray>("~/debug/marker", durable_qos);

   debug_msg_pub_ =
    create_publisher<tier4_debug_msgs::msg::StringStamped>("~/debug/calculation_time", 1);
   ```

   名前空間を設定して起動するとトピックが追加されるため、トピック名は次のようになります:

   `/planning/scenario_planning/lane_driving/motion_planning/obstacle_avoidance_planner/debug/marker /planning/scenario_planning/lane_driving/motion_planning/obstacle_avoidance_planner/debug/calculation_time`

4. 理論的根拠: トピック名を再割り当てし、起動ファイルから構成できるようにしたいと考えています。
