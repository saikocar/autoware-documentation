新しいノードを追加して評価する手順の例
概要
このページでは、新しいノードの実装時に Autoware を評価するためのガイド、特に新しいローカリゼーション ノードの開発について説明します。

ワークフローには、実車または AWSIM を使用した初期テストと rosbag の記録、新しいノードの実装、記録された rosbag を使用した後続のテスト、そして最後に実車または AWSSIM を使用した評価が含まれます。

追加する手法は公開データセット等で十分に検証されているものとします。

1. Autoware を標準構成で実行する
まず、パフォーマンスと動作を比較するための基礎を確立するために、標準の Autoware を実行できることが重要です。

Autoware には常に新しい機能が組み込まれています。現在のバージョンで期待どおりに動作することを最初に確認することが重要であり、問​​題のトラブルシューティングに役立ちます。

この文脈では、AWSSIM が想定されます。したがって、AWSSIM シミュレーターが役立ちます。実際のハードウェアを使用している場合は、ハウツー ガイドを参照してください。

2. Autoware を使用した rosbag の記録
新しいノードを開発する前に、評価のために rosbag を記録することをお勧めします。新しいセンサーが必要な場合は、車両または AWSIM に追加する必要があります。

この場合、トピックの要否に関わらず、すべてのトピックを保存することをお勧めします。たとえば、Localization では、初期位置推定サービスは rviz への入力と GNSS トピックによってトリガーされるため、これらのトピックが保存されていない限り、データの再生時に初期位置推定は開始されません。

データ容量が懸念される場合は、mcap 形式の使用を検討してください。

使用すると計算負荷が増加し、パフォーマンスに影響を与える可能性があることに注意してくださいros2 bag record。データを記録した後、センサー データのスムーズな流れと変更されていない時系列を検証することをお勧めします。この検証は、たとえば、rqt_image_viewwhileを使用して画像データを検査することによって実行できますros2 bag play。

3. 新しいノードの開発
新しいノードを開発するときは、作成しようとしているパッケージに似たパッケージを参照すると有益な場合があります。

「設計」ページをよく読み、Autoware でのノードの追加または置換を検討してから、ソリューションを実装することをお勧めします。

たとえば、LiDAR ベースの位置特定方法である NDT を実行するノードはndt_scan_matcherです。これを別のアプローチに置き換える場合は、同じトピックを生成し、同じサービスを提供するノードを実装します。

ndt_scan_matcherは、 pose_estimatorとして起動されるため、起動ファイルも置き換える必要があります。

4. rosbagベースのシミュレータによる評価
新しいノードが実装されたら、それを評価します。 logging_simulator は、ステップ 2 で取得した rosbag を使用して新しいノードを評価する方法を示すツールです。

logging_simulator を実行するときに、特定のコンポーネント ノードの起動を設定planning:=falseまたは無効にすることができます。control:=false

ros2 launch autoware_launch logging_simulator.launch.xml ... planning:=false control:=false

logging_simulator を起動した後、手順 2 で取得した rosbag ファイルを を使用して再生する必要がありますros2 bag play <rosbag_file>。

今回検証するローカリゼーションに関連するトピックを再マップすると、Autoware は記録したデータの代わりに今回計算しているデータを使用します。また、--topicsのオプションを使用するとros2 bag play、rosbag で特定のトピックのみを公開できます。

rosbag ファイルをフィルタリングし、必要なトピックのみを含む新しい rosbag ファイルを作成するために使用できるros2bag_extensionsがあります。

5. リアルタイム環境での評価
logging_simulator での動作を十分に確認したら、リアルタイム環境に新しいノードを追加して Autoware として実行してみましょう。

Autoware をデバッグするには、 debug-autowareで説明されている方法が便利です。

再現性を高めるために、GoalPose を修正することをお勧めします。このような場合は、tier4_automatic_goal_rviz_pluginの使用を検討してください。

6. 結果の共有
実装が正常に機能する場合は、Autoware へのプル リクエストを検討してください。

Show and Tellの Discussion で自分のアイデアを発表することから始めるのも良いでしょう。

ローカリゼーションに関しては、YabLoc の提案が貴重な洞察を提供する可能性があります。
# An example procedure for adding and evaluating a new node

## Overview

This page provides a guide for evaluating Autoware when a new node is implemented, especially about developing a novel localization node.

The workflow involves initial testing and rosbag recording using a real vehicle or AWSIM, implementing the new node, subsequent testing using the recorded rosbag, and finally evaluating with a real vehicle or AWSIM.

It is assumed that the method intended for addition has already been verified well with public datasets and so on.

## 1. Running Autoware in its standard configuration

First of all, it is important to be able to run the standard Autoware to establish a basis for performance and behavior comparison.

Autoware constantly incorporates new features.
It is crucial to initially confirm that it operates as expected with the current version, which helps in problem troubleshooting.

In this context, AWSIM is presumed.
Therefore, [AWSIM simulator](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/digital-twin-simulation/awsim-tutorial/) can be useful.
If you are using actual hardware, please refer to the [How-to guides](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/).

## 2. Recording a rosbag using Autoware

Before developing a new node, it is recommended to record a rosbag in order to evaluate.
If you need a new sensor, you should add it to your vehicle or AWSIM.

In this case, it is recommended to save all topics regardless of whether they are necessary or not.
For example, in Localization, since the initial position estimation service is triggered by the input to rviz and the GNSS topic, the initial position estimation does not start when playing back data unless those topics are saved.

Consider the use of the [mcap format](https://mcap.dev/) if data capacity becomes a concern.

It is worth noting that using `ros2 bag record` increases computational load and might affect performance.
After data recording, verifying the smooth flow of sensor data and unchanged time series is advised.
This verification can be accomplished, for example, by inspecting the image data with `rqt_image_view` during `ros2 bag play`.

## 3. Developing the new node

When developing a new node, it could be beneficial to reference a package that is similar to the one you intend to create.

It is advisable to thoroughly read the [Design page](https://autowarefoundation.github.io/autoware-documentation/main/design/), contemplate the addition or replacement of nodes in Autoware, and then implement your solution.

For example, a node doing NDT, a LiDAR-based localization method, is [ndt_scan_matcher](https://github.com/autowarefoundation/autoware.universe/tree/main/localization/ndt_scan_matcher).
If you want to replace this with a different approach, implement a node which produces the same topics and provides the same services.

`ndt_scan_matcher` is launched as [pose_estimator](https://github.com/autowarefoundation/autoware.universe/blob/main/launch/tier4_localization_launch/launch/pose_estimator/pose_estimator.launch.xml), so it is necessary to replace the launch file as well.

## 4. Evaluating by a rosbag-based simulator

Once the new node is implemented, it is time to evaluate it.
[logging_simulator](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/rosbag-replay-simulation/) is a tool of how to evaluate the new node using the rosbag captured in step 2.

When you run the logging_simulator, you can set `planning:=false` or `control:=false` to disable the launch of specific component nodes.

`ros2 launch autoware_launch logging_simulator.launch.xml ... planning:=false control:=false`

After launching logging_simulator, the rosbag file obtained in step 2 should be replayed using `ros2 bag play <rosbag_file>`.

If you remap the topics related to the localization that you want to verify this time, Autoware will use the data it is calculating this time instead of the data it recorded.
Also, using the `--topics` option of `ros2 bag play`, you can publish only specific topics in rosbag.

There is [ros2bag_extensions](https://github.com/tier4/ros2bag_extensions) available to filter the rosbag file and create a new rosbag file that contains only the topics you need.

## 5. Evaluating in a realtime environment

Once you have sufficiently verified the behavior in the logging_simulator, let's run it as Autoware with new nodes added in the realtime environment.

To debug Autoware, the method described at [debug-autoware](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/others/debug-autoware/) is useful.

For reproducibility, you may want to fix the GoalPose.
In such cases, consider using the [tier4_automatic_goal_rviz_plugin](https://github.com/autowarefoundation/autoware.universe/tree/main/common/tier4_automatic_goal_rviz_plugin).

## 6. Sharing the results

If your implementation works successfully, please consider a pull request to Autoware.

It is also a good idea to start by presenting your ideas in Discussion at [Show and tell](https://github.com/orgs/autowarefoundation/discussions/categories/show-and-tell).

For localization, [YabLoc's Proposal](https://github.com/orgs/autowarefoundation/discussions/3484) may provide valuable insights.
