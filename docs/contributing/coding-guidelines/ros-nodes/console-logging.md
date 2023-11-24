# コンソールの記録

ROS2ログはROSノードを理解してデバッグするための強力なツールです。

このページではAutowareでコンソールログを設計する方法に焦点を当ていくつかの実用的な例を示します。
ROS2ログの仕組みを包括的に理解するには[ログのドキュメント](https://docs.ros.org/en/humble/Concepts/About-Logging.html)を参照してください。

## Autowareでのログ記録の使用例

- 開発者はコンソールログを確認してコードをデバッグします。
- 車両のオペレーターはコンソールのログに応じて適切なリスク回避行動をとります。
- ログ解析者はrosbagファイルに記録されているコンソールログを分析します。

これらのユースケースを効率的にサポートするにはクリーンで視認性の高いログが必要です。
そのためにいくつかのルールを以下に定義します。

## ルール

### 適切な重大度レベルを選択します (必須、非自動化)

#### 理論的根拠

次のように重大度レベルが不適切だと混乱を招きます。:

- 役に立たないメッセージが`FATAL`としてマークされる。
- 非常に重要なエラー メッセージは`INFO`としてマークされています。

#### 例

次の基準を参考にしてください:

- **DEBUG:**  開発者向けにデバッグ情報を表示するにはこのレベルを使用します。このレベルのログはデフォルトでは非表示になることに注意してください。
- **INFO:** このレベルを使用して、イベント (初期化中の周期的な通知、状態変化、サービス応答など) をオペレーターに通知します。
- **WARN:** ノードは正常に動作し続けるが、意図しない動作が発生する可能性がある場合は、このレベルを使用します。
  - たとえば"パスの最適化は失敗したが、以前のデータは使用できる"、"位置推定スコアが低い"などです。
- **ERROR:** ノードが正常に動作し続けることができず、意図しない動作が発生する可能性がある場合は、このレベルを使用します。
  - たとえば"経路の最適化に失敗し、経路が空いている"、"車両が緊急停止する"などです。
- **FATAL:** システム全体が正しく動作し続けることができず、システムを停止する必要がある場合に、このレベルを使用します。
  - たとえば"車両制御ECUが応答しない"、"システム ストレージがクラッシュした"などです。

### ログオプションを設定して不要なログを除外する(必須、非自動)

#### 理論的根拠

ドライバーなどの一部のサードパーティノードはAutowareのガイドラインに従っていない場合があります。
ログにノイズが多い場合は、不要なログを除外する必要があります。

#### 例

`--log-level {level}`オプションを使用して、表示されるログの最小レベルを変更します:

```xml
<launch>
  <!-- This outputs only FATAL level logs. -->
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--log-level fatal" />
</launch>
```

特定の出力ターゲットのみを無効にする場合は、`--disable-stdout-logs`、`--disable-rosout-logs`および/または`--disable-external-lib-logs`オプションを使用します:

```xml
<launch>
  <!-- This outputs to rosout and disk. -->
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--disable-stdout-logs" />
</launch>
```

```xml
<launch>
  <!-- This outputs to stdout. -->
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--disable-rosout-logs --disable-external-lib-logs" />
</launch>
```

### ログが不必要に繰り返し表示される場合は、調整されたログを使用します(必須、非自動)

#### 理論的根拠

大量のログがコンソールに表示されると、重要なメッセージを見逃してしまいます。

#### 例

いくつかのメッセージを待機している間は、通常は調整されたログで十分です。
この場合は5秒程度を目安としてお待ちください。

```cpp
// 準拠
void FooNode::on_timer() {
  if (!current_pose_) {
    RCLCPP_ERROR_THROTTLE(get_logger(), *get_clock(), 5000, "Waiting for current_pose_.");
    return;
  }
}

// 非準拠
void FooNode::on_timer() {
  if (!current_pose_) {
    RCLCPP_ERROR(get_logger(), "Waiting for current_pose_.");
    return;
  }
}
```

#### 例外

以下の場合は、スロットルしていなくても許容されます。

- このメッセージは毎回表示する価値があります。
- メッセージレベルはDEBUGです。

### コアライブラリクラスのrclcpp::Node には依存せず、rclcpp/logging.hppのみに依存します(助言、非自動)

#### 理論的根拠

再利用可能なアルゴリズムを含むコアライブラリクラスは、非ROSプラットフォームでも使用できます。
ライブラリを他のプラットフォームに移植する場合は、依存関係が少ないことが優先されます。

#### 例

```cpp
// 準拠
#include <rclcpp/logging.hpp>

class FooCore {
public:
  explicit FooCore(const rclcpp::Logger & logger) : logger_(logger) {}

  void process() {
    RCLCPP_INFO(logger_, "message");
  }

private:
  rclcpp::Logger logger_;
};

// 準拠
// ロガー名がノード名と異なる場合、ログは`/rosout`に公開されないことに注意してください。
#include <rclcpp/logging.hpp>

class FooCore {
  void process() {
    RCLCPP_INFO(rclcpp::get_logger("foo_core_logger"), "message");
  }
};


// 非準拠
#include <rclcpp/node.hpp>

class FooCore {
public:
  explicit FooCore(const rclcpp::NodeOptions & node_options) : node_("foo_core_node", node_options) {}

  void process() {
    RCLCPP_INFO(node_.get_logger(), "message");
  }

private:
  rclcpp::Node node_;
};
```

## チップ

### rqt_consoleを使用してログをフィルタリングする

ログをフィルタリングするには`rqt_console`を使用するのが便利である:

```bash
ros2 run rqt_console rqt_console
```

詳細は[ROS2ドキュメント](https://docs.ros.org/en/rolling/Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.html)を参照してください。

### 便利なmarco（原文ママ）表現

プログラムをデバッグするには、どの関数やコード行が実行されるかを確認する必要がある場合があります。
その場合、`__FILE__`、 `__LINE__`、`__FUNCTION__`マクロを使用できます:

```cpp
void FooNode::on_timer() {
  RCLCPP_DEBUG(get_logger(), "file: %s, line: %s, function: %s" __FILE__, __LINE__, __FUNCTION__);
}
```

出力例は次のとおりです:

> [DEBUG] [1671720414.395456931] [foo]: file: /path/to/file.cpp, line: 100, function: on_timer
