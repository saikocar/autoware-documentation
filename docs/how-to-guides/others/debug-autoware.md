オートウェアのデバッグ
このページでは、Autoware をデバッグするためのいくつかの方法を説明します。

デバッグメッセージを出力する
デバッグに不可欠なことは、プログラムの動作を迅速に判断し、問題を特定できるように、プログラム情報を明確に出力することです。Autoware は、ROS 2 ログ ツールを使用してデバッグ メッセージを出力します。コンソール ログの設計方法については、チュートリアル「コンソール ログ」を参照してください。

ROS ツールを使用して Autoware をデバッグする
コマンドラインツールの使用
ROS 2 には、ROS 2 システムをイントロスペクトするための一連のコマンドライン ツールが含まれています。ツールの主なエントリ ポイントは command です。ros2このコマンド自体には、ノード、トピック、サービスなどをイントロスペクトして操作するためのさまざまなサブコマンドがあります。ROS 2 コマンド ライン ツールの使用方法については、チュートリアルCLI ツールを参照してください。

rviz2 の使用
Rviz2 は、Rviz を ROS 2 に移植したものです。ユーザーがロボット、センサー データ、マップなどを表示するためのグラフィカル インターフェイスを提供します。Rviz2 ツールは次の方法で簡単に実行できます。

rviz2
Autoware がシミュレーターを起動すると、デフォルトで Rviz2 ツールが開き、オートパイロットのグラフィック情報が視覚化されます。

RQT ツールの使用
RQt は、プラグインの形式でさまざまなツールやインターフェイスを実装するグラフィカル ユーザー インターフェイス フレームワークです。RQt ツール/プラグインは次の方法で簡単に実行できます。

rqt
この GUI を使用すると、システム上で利用可能なプラグインを選択できます。スタンドアロン ウィンドウでプラグインを実行することもできます。たとえば、RQt コンソール:

ros2 run rqt_console rqt_console
一般的な RQt ツール
rqt_graph: ノードの相互作用を表示する

複雑なアプリケーションでは、ROS ノードの相互作用を視覚的に表現すると役立つ場合があります。

ros2 run rqt_graph rqt_graph
rqt_console: メッセージの表示

rqt_console は、ROS トピックを表示するための優れた GUI です。

ros2 run rqt_console rqt_console
rqt_plot: データプロットを表示する

rqt_plot は、ROS データをリアルタイムでプロットする簡単な方法です。

ros2 run rqt_plot rqt_plot
ros2_graphの使用
ros2_graphマークダウン ファイルに追加する ROS 2 グラフのマーメイド記述を生成するために使用できます。

rqt_graph生成された人魚図をレンダリングするために何らかのツールが必要になる場合でも、カラフルな代替手段として使用することもできます。

以下でインストールできます。

pip install ros2-graph
次に、次のようにしてグラフの人魚の説明を生成できます。

ros2_graph your_node

# or like with an output file
ros2_graph /turtlesim -o turtle_diagram.md

# or multiple nodes
ros2_graph /turtlesim /teleop_turtle
次に、これらのグラフを次のように視覚化できます。

マーメイドライブエディター
Visual Studio Code 拡張機能マーメイド プレビュー
ネイティブサポートを備えたJetBrains IDE
ros2doctorの使用
ROS 2 セットアップが期待どおりに実行されない場合は、ros2doctorツールを使用してその設定を確認できます。

ros2doctorプラットフォーム、バージョン、ネットワーク、環境、実行中のシステムなどを含む ROS 2 のあらゆる側面をチェックし、考えられるエラーや問題の理由について警告します。

ros2 doctorターミナルで実行するだけで簡単です。

システム内のすべてのトピックについて「パブリッシャーのないサブスクライバー」をリストする機能があります。

この情報は、必要なノードが実行中でないかどうかを見つけるのに役立ちます。

詳細については、「 ros2doctor を使用して問題を特定する 」に関する次の公式ドキュメントを参照してください。

ブレークポイントを使用したデバッガの使用
多くの IDE (Visual Studio Code、CLion など) は、Linux プラットフォーム上の GBD を使用した C/C++ 実行可能ファイルのデバッグをサポートしています。以下に、デバッガーの使用に関する参考資料をいくつか示します。

https://code.visualstudio.com/docs/cpp/cpp-debug
https://www.jetbrains.com/help/clion/debugging-code.html#useful-debugger-shortcuts
# Debug Autoware

This page provides some methods for debugging Autoware.

## Print debug messages

The essential thing for debug is to print the program information clearly, which can quickly judge the program operation and locate the problem. Autoware uses ROS 2 logging tool to print debug messages, how to design console logging refer to tutorial [Console logging](../../contributing/coding-guidelines/ros-nodes/console-logging.md).

## Using ROS tools debug Autoware

### Using command line tools

ROS 2 includes a suite of command-line tools for introspecting a ROS 2 system. The main entry point for the tools is the command `ros2`, which itself has various sub-commands for introspecting and working with nodes, topics, services, and more. How to use the ROS 2 command line tool refer to tutorial [CLI tools](http://docs.ros.org/en/galactic/Tutorials/Beginner-CLI-Tools.html).

### Using rviz2

Rviz2 is a port of Rviz to ROS 2. It provides a graphical interface for users to view their robot, sensor data, maps, and more. You can run Rviz2 tool easily by:

```console
rviz2
```

When Autoware launch the simulators, the Rviz2 tool is opened by default to visualize the autopilot graphic information.

### Using rqt tools

RQt is a graphical user interface framework that implements various tools and interfaces in the form of plugins. You can run any RQt tools/plugins easily by:

```console
rqt
```

This GUI allows you to choose any available plugins on your system. You can also run plugins in standalone windows. For example, RQt Console:

```console
ros2 run rqt_console rqt_console
```

#### Common RQt tools

1. rqt_graph: view node interaction

   In complex applications, it may be helpful to get a visual representation of the ROS node interactions.

   ```console
   ros2 run rqt_graph rqt_graph
   ```

2. rqt_console: view messages

   rqt_console is a great gui for viewing ROS topics.

   ```console
   ros2 run rqt_console rqt_console
   ```

3. rqt_plot: view data plots

   rqt_plot is an easy way to plot ROS data in real time.

   ```console
   ros2 run rqt_plot rqt_plot
   ```

### Using ros2_graph

[`ros2_graph`](https://github.com/kiwicampus/ros2_graph) can be used to generate [mermaid](https://mermaid.js.org/#/) description of ROS 2 graphs to add on your markdown files.

It can also be used as a colorful alternative to `rqt_graph` even though it would require some tool to render the generated mermaid diagram.

It can be installed with:

```bash
pip install ros2-graph
```

Then you can generate a mermaid description of the graph with:

```bash
ros2_graph your_node

# or like with an output file
ros2_graph /turtlesim -o turtle_diagram.md

# or multiple nodes
ros2_graph /turtlesim /teleop_turtle
```

You can then visualize these graphs with:

- [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/)
- Visual Studio Code extension [mermaid preview](https://marketplace.visualstudio.com/items?itemName=vstirbu.vscode-mermaid-preview)
- JetBrains IDEs [with native support](https://www.jetbrains.com/go/guide/tips/mermaid-js-support-in-markdown/)

### Using ros2doctor

When your ROS 2 setup is not running as expected, you can check its settings with the `ros2doctor` tool.

`ros2doctor` checks all aspects of ROS 2, including platform, version, network, environment, running systems and more, and warns you about possible errors and reasons for issues.

It's as simple as just running `ros2 doctor` in your terminal.

It has the ability to list "Subscribers without publishers" for all topics in the system.

And this information can help you find if a necessary node isn't running.

For more details, see the following official documentation for [Using ros2doctor to identify issues](https://docs.ros.org/en/rolling/Tutorials/Beginner-Client-Libraries/Getting-Started-With-Ros2doctor.html).

## Using a debugger with breakpoints

Many IDE(e.g. Visual Studio Code, CLion) supports debugging C/C++ executable with GBD on linux platform. The following lists some references for using the debugger:

- <https://code.visualstudio.com/docs/cpp/cpp-debug>
- <https://www.jetbrains.com/help/clion/debugging-code.html#useful-debugger-shortcuts>
