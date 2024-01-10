# 新しいノードを追加して評価する手順の例

## 概要

このページでは、新しいノードの実装時にAutowareを評価するためのガイド、特に新しい位置推定ノードの開発について説明します。

ワークフローには、実車またはAWSIMを使用した初期テストとrosbagの記録、新しいノードの実装、記録されたrosbagを使用した後続のテスト、そして最後に実車またはAWSIMを使用した評価が含まれます。

追加する手法は公開データセット等で十分に検証されているものとします。

## 1. Autowareを標準構成で実行する

まず、パフォーマンスと動作を比較するための基礎を確立するために、標準の Autoware を実行できることが重要です。

Autowareには常に新しい機能が組み込まれています。
現在のバージョンで期待どおりに動作することを最初に確認することが重要であり、問​​題のトラブルシューティングに役立ちます。

この文脈では、AWSIMが想定されます。
したがって、[AWSIMシミュレーター](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/digital-twin-simulation/awsim-tutorial/)が役立ちます。
実際のハードウェアを使用している場合は、[ハウツーガイド](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/)を参照してください。

## 2. Autowareを使用したrosbagの記録

新しいノードを開発する前に、評価のためにrosbagを記録することをお勧めします。
新しいセンサーが必要な場合は、車両またはAWSIMに追加する必要があります。

この場合、トピックの要否に関わらず、すべてのトピックを保存することをお勧めします。
たとえば、位置推定では、初期位置推定サービスはrvizへの入力とGNSSトピックによってトリガーされるため、これらのトピックが保存されていない限り、データの再生時に初期位置推定は開始されません。

データ容量が懸念される場合は、[mcap方式](https://mcap.dev/)の使用を検討してください。

`ros2 bag record`を使用すると計算負荷が増加し、パフォーマンスに影響を与える可能性があることに注意してください。
データを記録した後、センサーデータのスムーズな流れと変更されていない時系列を検証することをお勧めします。
この検証は、たとえば、`rqt_image_view`を使用して`ros2 bag play`の画像データを検査することによって実行できます。

## 3. 新しいノードの開発

新しいノードを開発するときは、作成しようとしているパッケージに似たパッケージを参照すると有益な場合があります。

[設計ページ](https://autowarefoundation.github.io/autoware-documentation/main/design/)をよく読み、Autoware でのノードの追加または置換を検討してから、ソリューションを実装することをお勧めします。

たとえば、LiDARベースの位置特定方法であるNDTを実行するノードは[ndt_scan_matcher](https://github.com/autowarefoundation/autoware.universe/tree/main/localization/ndt_scan_matcher)です。
これを別のアプローチに置き換える場合は、同じトピックを生成し、同じサービスを提供するノードを実装します。

`ndt_scan_matcher`は[pose_estimator](https://github.com/autowarefoundation/autoware.universe/blob/main/launch/tier4_localization_launch/launch/pose_estimator/pose_estimator.launch.xml)として起動されるため、起動ファイルも置き換える必要があります。

## 4. rosbagベースのシミュレータによる評価

新しいノードが実装されたら、それを評価します。
[logging_simulator](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/rosbag-replay-simulation/)、ステップ2で取得したrosbagを使用して新しいノードを評価する方法を示すツールです。

logging_simulatorを実行するときに、特定のコンポーネントノードの起動を`planning:=false`または`control:=false`に設定することで無効にすることができます。

`ros2 launch autoware_launch logging_simulator.launch.xml ... planning:=false control:=false`

logging_simulatorを起動した後、手順2で取得したrosbagファイルを`ros2 bag play <rosbag_file>`を使用して再生する必要があります。

今回検証する位置推定に関連するトピックを再マップすると、Autowareは記録したデータの代わりに今回計算しているデータを使用します。
また、 `--topics`のオプションを使用すると`ros2 bag play`、でrosbagの特定のトピックのみを公開できます。 

rosbagファイルをフィルタリングし、必要なトピックのみを含む新しいrosbagファイルを作成するために使用できる[ros2bag_extensions](https://github.com/tier4/ros2bag_extensions) があります。

## 5. リアルタイム環境での評価

logging_simulatorでの動作を十分に確認したら、リアルタイム環境に新しいノードを追加して Autoware として実行してみましょう。

Autowareをデバッグするには、[debug-autoware](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/others/debug-autoware/)で説明されている方法が便利です。

再現性を高めるために、GoalPoseを修正することをお勧めします。
このような場合は[tier4_automatic_goal_rviz_plugin](https://github.com/autowarefoundation/autoware.universe/tree/main/common/tier4_automatic_goal_rviz_plugin)の使用を検討してください。

## 6. 結果の共有

実装が正常に機能する場合は、Autowareへのプルリクエストを検討してください。

[Show and tell](https://github.com/orgs/autowarefoundation/discussions/categories/show-and-tell)のDiscussionで自分のアイデアを発表することから始めるのも良いでしょう。

位置推定については[YabLocの提案](https://github.com/orgs/autowarefoundation/discussions/3484)が貴重な洞察を提供する可能性があります。
