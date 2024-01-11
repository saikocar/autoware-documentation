# Autowareのデバッグ

このページでは、Autowareをデバッグするためのいくつかの方法を説明します。

## デバッグメッセージを出力する

デバッグに不可欠なことは、プログラムの動作を迅速に判断し、問題を特定できるように、プログラム情報を明確に出力することです。Autowareは、ROS 2ログツールを使用してデバッグメッセージを出力します。コンソールログの設計方法については、チュートリアル[コンソールログ](../../contributing/coding-guidelines/ros-nodes/console-logging.md)を参照してください。

## ROSツールを使用して Autowareをデバッグする

### コマンドラインツールの使用

ROS2には、ROS2システムをイントロスペクトするための一連のコマンドラインツールが含まれています。ツールの主なエントリポイントは`ros2`コマンドです。このコマンド自体には、ノード、トピック、サービスなどをイントロスペクトして操作するためのさまざまなサブコマンドがあります。ROS 2 コマンド ライン ツールの使用方法については、チュートリア[CLIツール](http://docs.ros.org/en/galactic/Tutorials/Beginner-CLI-Tools.html)を参照してください。

### rviz2の使用

Rviz2は、RvizをROS2に移植したものです。ユーザーがロボット、センサーデータ、マップなどを表示するためのグラフィカルインターフェイスを提供します。Rviz2ツールは次の方法で簡単に実行できます:

```console
rviz2
```

Autowareがシミュレーターを起動すると、デフォルトでRviz2ツールが開き、オートパイロットのグラフィック情報が視覚化されます。

### rqtツールの使用

RQtは、プラグインの形式でさまざまなツールやインターフェイスを実装するグラフィカルユーザーインターフェイスフレームワークです。RQt ツール/プラグインは次の方法で簡単に実行できます:

```console
rqt
```

このGUIを使用すると、システム上で利用可能なプラグインを選択できます。スタンドアロンウィンドウでプラグインを実行することもできます。たとえば、RQtコンソール:

```console
ros2 run rqt_console rqt_console
```

#### 一般的なRQtツール

1. rqt_graph: ノードの相互作用を表示する

   複雑なアプリケーションでは、ROSノードの相互作用を視覚的に表現すると役立つ場合があります。

   ```console
   ros2 run rqt_graph rqt_graph
   ```

2. rqt_console: メッセージの表示

   rqt_consoleは、ROSトピックを表示するための優れたGUIです。

   ```console
   ros2 run rqt_console rqt_console
   ```

3. rqt_plot: データプロットを表示する

   rqt_plotは、ROSデータをリアルタイムでプロットする簡単な方法です。

   ```console
   ros2 run rqt_plot rqt_plot
   ```

### ros2_graphの使用

[`ros2_graph`](https://github.com/kiwicampus/ros2_graph)はマークダウンファイルに追加するROS 2 グラフの[マーメイド](https://mermaid.js.org/#/)記述を生成するために使用できます。

生成されたマーメイド図をレンダリングするために何らかのツールが必要になる場合でも、`rqt_graph`のカラフルな代替手段として使用することもできます。

以下でインストールできます:

```bash
pip install ros2-graph
```

次に、次のようにしてグラフのマーメイドの説明を生成できます:

```bash
ros2_graph your_node

# またはファイル出力による
ros2_graph /turtlesim -o turtle_diagram.md

# または複数のノードに渡る
ros2_graph /turtlesim /teleop_turtle
```

次に、これらのグラフを以下のツールで視覚化できます:

- [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/)
- Visual Studio Code extension [mermaid preview](https://marketplace.visualstudio.com/items?itemName=vstirbu.vscode-mermaid-preview)
- JetBrains IDEsの[ネイティブサポート](https://www.jetbrains.com/go/guide/tips/mermaid-js-support-in-markdown/)

### ros2doctorの使用

ROS 2 セットアップが期待どおりに実行されない場合は、`ros2doctor`ツールを使用してその設定を確認できます。

`ros2doctor`はプラットフォーム、バージョン、ネットワーク、環境、実行中のシステムなどを含む ROS 2 のあらゆる側面をチェックし、考えられるエラーや問題の理由について警告します。

`ros2 doctor`ターミナルで`ros2 doctor`を実行するだけで簡単です。

システム内のすべてのトピックについて"パブリッシャーのないサブスクライバー"をリストする機能があります。

この情報は、必要なノードが実行中でないかどうかを見つけるのに役立ちます。

詳細については、[ros2doctorを使用して問題を特定する](https://docs.ros.org/en/rolling/Tutorials/Beginner-Client-Libraries/Getting-Started-With-Ros2doctor.html)に関する次の公式ドキュメントを参照してください。

## ブレークポイントを使用したデバッガの使用

多くのIDE(Visual Studio Code、CLion など)は、Linuxプラットフォーム上のGBDを使用したC/C++実行可能ファイルのデバッグをサポートしています。以下に、デバッガーの使用に関する参考資料をいくつか示します:

- <https://code.visualstudio.com/docs/cpp/cpp-debug>
- <https://www.jetbrains.com/help/clion/debugging-code.html#useful-debugger-shortcuts>
