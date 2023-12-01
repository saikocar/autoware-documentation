# 結合テスト

統合テストは個々のソフトウェアモジュールを組み合わせてグループとしてテストするソフトウェアテストのフェーズとして定義されます。
統合テストは単体テストの後、検証テストの前に行われます。

統合テストへの入力は単体テストが行​​われた独立したモジュールのセットです。
モジュールのセットは定義された統合テスト計画に対してテストされ、出力はシステムテストの準備ができた、
適切に統合されたソフトウェア モジュールのセットになります。

## 統合テストの価値

統合テストでは、個別に開発されたソフトウェアモジュールが相互に接続されたときに正しく動作するかどうかを判断します。
ROS2ではソフトウェアモジュールはノードと呼ばれます。
単一ノードのテストは一般にコンポーネントテストと呼ばれる特殊なタイプの統合テストです。

統合テストは次の種類のエラーを見つけるのに役立ちます:

- ノード間の互換性のない対話(トピックの不一致、異なるメッセージタイプ、互換性のないQoS設定など)。
- 単体テストでは触れられなかったエッジケース。重大なタイミング問題、ネットワーク通信の遅延、ディスクI/O障害、実稼働環境で発生する可能性のあるその他の問題など。
- システムのCPUあるいはメモリ負荷が高いときに発生する可能性のある問題 (`malloc`エラーなど)。これは、`stress`や`udpreplay`などのツールを使用してテストし、実際のデータでノードのパフォーマンスをテストできます。

ROS2を使用すると、多数のノードを含む複雑な自動運転アプリケーションをプログラムすることができます。
したがって、開発者がROS2ノードの相互作用をテストするのに役立つ統合テストフレームワークを提供するために、多大な努力が払われてきました。

## 統合テストフレームワーク

一般的な統合テストフレームワークには、次の3つの部品があります:

1. 連携して出力を生成する引数を持つ一連の実行可能ファイル。
2. 実行可能ファイルの出力と一致する必要がある一連の予期される出力。
3. テストを開始し、出力を予想される出力と比較し、テストが合格したかどうかを判断するランチャー。

Autowareでは[launch_testing](https://github.com/ros2/launch/tree/master/launch_testing)フレームワークを使用します。

### スモークテスト

Autowareにはスモークテスト用の専用APIがあります。
このフレームワークを使用するには、`package.xml`に以下を追加します:

```xml
<test_depend>autoware_testing</test_depend>
```

そして`CMakeLists.txt`に以下を追加します:

```cmake
if(BUILD_TESTING)
  find_package(autoware_testing REQUIRED)
  add_smoke_test(${PROJECT_NAME} ${NODE_NAME})
endif()
```

これによりノードが次の状態にあることを確認するスモークテストが追加されます:

1. デフォルトのパラメータファイルを使用して起動します。
2. 標準の`SIGTERM`シグナルで終了します。

完全なAPIドキュメントについては[パッケージ設計ページ](https://github.com/autowarefoundation/autoware.universe/blob/main/common/autoware_testing/design/autoware_testing-design.md)を参照してください。

!!! 注記

    このAPIはすべてのスモークテストケースに適しているわけではありません。
    特定のファイルの場所 (マップなど) をノードに渡す必要がある場合、またはノードの起動前に何らかの準備を行う必要がある場合は使用できません。
    このような場合は[以下のコンポーネント テストセクション](#integration-test-with-a-single-node-component-test)の手動ソリューションを使用してください。

### 単一ノードでの統合テスト: コンポーネントテスト

最も単純なシナリオは単一ノードです。
この場合、統合テストは一般にコンポーネントテストと呼ばれます。

コンポーネントテストを既存のノードに追加するには[`map_loader`パッケージ](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_loader)
([このPR](https://github.com/autowarefoundation/autoware.universe/pull/1056)に追加)の`lanelet2_map_loader`の例に従うことができます。

[`package.xml`](https://github.com/autowarefoundation/autoware.universe/blob/main/map/map_loader/package.xml)に以下を追加します:

```xml
<test_depend>ros_testing</test_depend>
```

[`CMakeLists.txt`](https://github.com/autowarefoundation/autoware.universe/blob/main/map/map_loader/CMakeLists.txt)で,
`BUILD_TESTING`セクションを追加または変更します:

```cmake
if(BUILD_TESTING)
  add_ros_test(
    test/lanelet2_map_loader_launch.test.py
    TIMEOUT "30"
  )
  install(DIRECTORY
    test/data/
    DESTINATION share/${PROJECT_NAME}/test/data/
  )
endif()
```

コマンド`add_ros_test`に加えて、`install`コマンドを使用してテストに必要なデータもインストールします。

!!! 注記

    - `TIMEOUT`引数は秒単位で指定します。詳細については、[add_ros_test.cmakeファイル](https://github.com/ros2/ros_testing/blob/master/ros_testing/cmake/add_ros_test.cmake)を参照してください。
    - `add_ros_test`コマンドは、並行して実行されるテスト間の干渉を避けるため、一意の`ROS_DOMAIN_ID`でテストを実行します。

テストを作成するには[launch_testingクイックスタート例](https://github.com/ros2/launch/tree/master/launch_testing#quick-start-example)を読むか、
以下の手順に従います。or follow the steps below.

[`test/lanelet2_map_loader_launch.test.py`](https://github.com/autowarefoundation/autoware.universe/blob/main/map/map_loader/test/lanelet2_map_loader_launch.test.py)を例として、
最初の依存関係がインポートされます:

```python
import os
import unittest

from ament_index_python import get_package_share_directory
import launch
from launch import LaunchDescription
from launch_ros.actions import Node
import launch_testing
import pytest
```

次に、テスト対象のノードを起動するための起動説明が作成されます。
[`test_map.osm`](https://github.com/autowarefoundation/autoware.universe/blob/main/map/map_loader/test/data/test_map.osm) ファイルパスが検出され、ノードに渡されることに注意してください。
これは[スモークテストAPI](#smoke-tests)では実行できません:

```python
@pytest.mark.launch_test
def generate_test_description():

    lanelet2_map_path = os.path.join(
        get_package_share_directory("map_loader"), "test/data/test_map.osm"
    )

    lanelet2_map_loader = Node(
        package="map_loader",
        executable="lanelet2_map_loader",
        parameters=[{"lanelet2_map_path": lanelet2_map_path}],
    )

    context = {}

    return (
        LaunchDescription(
            [
                lanelet2_map_loader,
                # Start test after 1s - gives time for the map_loader to finish initialization
                launch.actions.TimerAction(
                    period=1.0, actions=[launch_testing.actions.ReadyToTest()]
                ),
            ]
        ),
        context,
    )
```

!!! 注記

    - ノードが入力のlanelet2マップを処理するのに時間がかかるため、`TimerAction`を使用してテストの開始を1秒遅らせます。
    - 上の例では`context`は空ですがオブジェクトをテストケースに渡すために使用できます。
    - `context`の使用例は、[ROS 2 context_launch_test.py](https://github.com/ros2/launch/blob/humble/launch_testing/test/launch_testing/examples/context_launch_test.py)のテスト例で見つけることができます。

最後にノードの実行可能ファイルがシャットダウンされた後にテストが実行されます (`post_shutdown_test`)。
ここではノードがエラーなく起動され、正常に終了したことを確認します。

```python
@launch_testing.post_shutdown_test()
class TestProcessOutput(unittest.TestCase):
    def test_exit_code(self, proc_info):
        # Check that process exits with code 0: no error
        launch_testing.asserts.assertExitCodes(proc_info)
```

## テストの実行

上記の例を続けて、最初にパッケージをビルドします:

```console
colcon build --packages-up-to map_loader
source install/setup.bash
```

次に、コンポーネントテストを手動で実行します。:

```console
ros2 test src/universe/autoware.universe/map/map_loader/test/lanelet2_map_loader_launch.test.py
```

または、パッケージ全体のテストの一環として:

```console
colcon test --packages-select map_loader
```

テストが実行されたことを確認します。例えば

```console
$ colcon test-result --all --verbose
...
build/map_loader/test_results/map_loader/test_lanelet2_map_loader_launch.test.py.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped
```

### 次の手順

[単一ノードでの統合テスト: コンポーネントテスト](#integration-test-with-a-single-node-component-test)で説明されている単純なテストは、ノードの出力のテストなど、さまざまな方向に拡張できます。

#### ノードの出力のテスト

ノードの実行中にテストするには、Pythonの`unittest.TestCase`のサブクラスを`*launch.test.py`に追加して[アクティブテスト](https://github.com/ros2/launch/tree/foxy/launch_testing#active-tests)を作成します。
ノードと特定のトピックへのサブスクリプションを作成して出力にアクセスするには、いくつかのボイラープレートコードが必要です。例えば

```python
import unittest

class TestRunningDataPublisher(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        cls.context = Context()
        rclpy.init(context=cls.context)
        cls.node = rclpy.create_node("test_node", context=cls.context)

    @classmethod
    def tearDownClass(cls):
        rclpy.shutdown(context=cls.context)

    def setUp(self):
        self.msgs = []
        sub = self.node.create_subscription(
            msg_type=my_msg_type,
            topic="/info_test",
            callback=self._msg_received
        )
        self.addCleanup(self.node.destroy_subscription, sub)

    def _msg_received(self, msg):
        # Callback for ROS 2 subscriber used in the test
        self.msgs.append(msg)

    def get_message(self):
        startlen = len(self.msgs)

        executor = rclpy.executors.SingleThreadedExecutor(context=self.context)
        executor.add_node(self.node)

        try:
            # Try up to 60 s to receive messages
            end_time = time.time() + 60.0
            while time.time() < end_time:
                executor.spin_once(timeout_sec=0.1)
                if startlen != len(self.msgs):
                    break

            self.assertNotEqual(startlen, len(self.msgs))
            return self.msgs[-1]
        finally:
            executor.remove_node(self.node)

    def test_message_content():
        msg = self.get_message()
        self.assertEqual(msg, "Hello, world")
```

## 参考

- [colcon](https://github.com/ros2/ros2/wiki/Colcon-Tutorial) はテストの構築と実行に使用されます。
- [launch testing](https://github.com/ros2/launch/tree/master/launch_testing)はノードを起動し、テストを実行します。
- [テストのガイドライン](index.md)ではAutowareで実行されるさまざまな種類のテストについて説明し、対応するガイドラインへのリンクを示します。
