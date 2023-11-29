# パラメーター

Autoware ROSノードは宣言されたパラメーターを持ち、その値はノードの起動時にパラメーターファイルの形式で提供されます。対応する値を持つすべての予期されるパラメータがパラメータファイルに存在する必要があります。アプリケーションによってはパラメータ値の変更が必要になる場合があります。

パラメーターの詳細についてはROSの公式ドキュメントを参照してください:

- [ROS2パラメータを理解する](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.html)
- [ROS2パラメータについて](https://docs.ros.org/en/humble/Concepts/About-ROS-2-Parameters.html)

## ワークフロー

[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)関数を使用するROSパッケージは次のことを行う必要があります:

- デフォルト値を指定せずに[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)を使用する
- パラメータファイルを作成する
- スキーマファイルを作成する

このワークフローの背後にある理論的根拠は、ROSノードに渡し、Webドキュメントで使用する検証済みの信頼できる単一ソースを用意することです。このアプローチにより、無効なパラメータ値を使用するリスクが軽減され、ドキュメントのメンテナンスが容易になります。これは次のようにして実現されます:

- パラメータ ファイルに予期されたパラメータが存在しない場合[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)は例外をスローします。
- 以下の図に示すように、スキーマはCI内のパラメータファイルを検証し、パラメータテーブルをレンダリングします。

  ```mermaid
  flowchart TD
      NodeSchema[Schema file: *.schema.json]
      ParameterFile[Parameter file: *.param.yaml]
      WebDocumentation[Web documentation table]

      NodeSchema -->|Validation| ParameterFile
      NodeSchema -->|Generate| WebDocumentation
  ```

注記: 実行時に検証が行われないため、パラメータ値を変更して検証を回避することができます。

## パラメータ関数の宣言

ノードの起動時にパラメータ値を設定するのは[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)関数です。

```cpp
declare_parameter<INSERT_TYPE>("INSERT_PARAMETER_1_NAME"),
declare_parameter<INSERT_TYPE>("INSERT_PARAMETER_N_NAME")
```

_default_value_が提供されていないため、提供された`*.param.yaml`ファイルにパラメーターが欠落している場合、関数は例外をスローします。[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)関数には、_INSERT_TYPE_を置き換えて、以下の表の_C++ Type_列の型を使用します。

| パラメータ型の列挙        | C++型                   |
| ------------------------- | -------------------------- |
| `PARAMETER_BOOL`          | `bool`                     |
| `PARAMETER_INTEGER`       | `int64_t`                  |
| `PARAMETER_DOUBLE`        | `double`                   |
| `PARAMETER_STRING`        | `std::string`              |
| `PARAMETER_BYTE_ARRAY`    | `std::vector<uint8_t>`     |
| `PARAMETER_BOOL_ARRAY`    | `std::vector<bool>`        |
| `PARAMETER_INTEGER_ARRAY` | `std::vector<int64_t>`     |
| `PARAMETER_DOUBLE_ARRAY`  | `std::vector<double>`      |
| `PARAMETER_STRING_ARRAY`  | `std::vector<std::string>` |

この表は[パラメータの型](https://github.com/ros2/rcl_interfaces/blob/humble/rcl_interfaces/msg/ParameterType.msg)と[パラメータの値](https://github.com/ros2/rcl_interfaces/blob/humble/rcl_interfaces/msg/ParameterValue.msg)から派生しています。

例を確認します: _Lidar Apollo Segmentation TVM Nodes_ [declare function](https://github.com/autowarefoundation/autoware.universe/blob/f85c90b56ed4c7d6b52e787570e590cff786b28b/perception/lidar_apollo_segmentation_tvm_nodes/src/lidar_apollo_segmentation_tvm_node.cpp#L38)

## パラメータファイル

パラメータファイル
説明やタイプなどの追加情報をユーザーに提供する必要がないため、パラメータファイルは最小限です。これは関連するスキーマファイルが追加情報を提供するためです。 ROSノードの開始点として以下のテンプレートを使用します。

```yaml
/**:
  ros__parameters:
    INSERT_PARAMETER_1_NAME: INSERT_PARAMETER_1_VALUE
    INSERT_PARAMETER_N_NAME: INSERT_PARAMETER_N_VALUE
```

注記: `/**`は明示的なノード名前空間の代わりに使用されます。これにより[再マップ](https://design.ros2.org/articles/static_remapping.html)されたROS ノードにパラメータファイルを渡すことができます。

テンプレートをROSノードに適合させるには、すべてのパラメーターの`INSERT_PARAMETER_..._NAME`と `INSERT_PARAMETER_..._VALUE`をそれぞれ置き換えます。各[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)は入力として1つのパラメータを受け取ります。自動フォーマットを適切に適用できるようにすべてのパラメーターファイルには`.param.yaml`接尾辞が必要です。

AutowareにはROSパッケージ用の次の2種類のパラメータファイルがあります:

- **ノードパラメータファイル**
  - ノードパラメータファイルにはAutowareの各パッケージに提供されるデフォルトパラメータが保存されます。
    - 例えば[`behavior_path_planner`のパラメータ](https://github.com/autowarefoundation/autoware.universe/tree/245242cee866de2d113e89c562353c5fc17f1f98/planning/behavior_path_planner/config)
  - ROSパラメータがノード内で宣言されている場合Autowareのすべてのノードにパラメータファイルが必要です。
  - `FOO_package`の場合、パラメータは`FOO_package/config`に保存されることが想定されます。
  - 個々のパッケージの起動ファイルはデフォルトでノードパラメータをロードする必要があります:

```xml
<launch>
  <arg name="foo_node_param_path" default="$(find-pkg-share FOO_package)/config/foo_node.param.yaml" />

  <node pkg="FOO_package" exec="foo_node">
    ...
    <param from="$(var foo_node_param_path)" />
  </node>
</launch>
```

- **起動パラメータファイル**
  - ユーザーが自分の車両の起動パッケージを作成するとき、ユーザーは起動ファイル内で"launch parameter files"として呼び出されるノードのノードパラメータファイルをコピーする必要があります。
  - その後、起動パラメータファイルがユーザーの車両に合わせて特別にカスタマイズされます。
    - 例えば[`autoware_launch`配下に登録されているカスタマイズされた`behavior_path_planner`のパラメータ](https://github.com/autowarefoundation/autoware_launch/tree/5fa613b9d80bf4f0db77efde03a43f7ede6bac86/autoware_launch/config)
  - 起動パラメータファイルの例は`autoware_launch`に保存されています。

## JSONスキーマ

[JSONスキーマ](https://json-schema.org/understanding-json-schema/index.html)は、パラメータファイルを検証して、ファイルの構造と内容が正しいことを確認するために使用されます。この目的でJSONスキーマを使用することは、クラウドネイティブ開発のベストプラクティスと考えられます。以下のスキーマテンプレートは、ROSノードのスキーマを定義する際の開始点として使用されます。

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "INSERT_TITLE",
  "type": "object",
  "definitions": {
    "INSERT_ROS_NODE_NAME": {
      "type": "object",
      "properties": {
        "INSERT_PARAMETER_1_NAME": {
          "type": "INSERT_TYPE",
          "description": "INSERT_DESCRIPTION",
          "default": "INSERT_DEFAULT",
          "INSERT_BOUND_CONDITION(S)": INSERT_BOUND_VALUE(S)
        },
        "INSERT_PARAMETER_N_NAME": {
          "type": "INSERT_TYPE",
          "description": "INSERT_DESCRIPTION",
          "default": "INSERT_DEFAULT",
          "INSERT_BOUND_CONDITION(S)": INSERT_BOUND_VALUE(S)
        }
      },
      "required": ["INSERT_PARAMETER_1_NAME", "INSERT_PARAMETER_N_NAME"],
      "additionalProperties": false
    }
  },
  "properties": {
    "/**": {
      "type": "object",
      "properties": {
        "ros__parameters": {
          "$ref": "#/definitions/INSERT_ROS_NODE_NAME"
        }
      },
      "required": ["ros__parameters"],
      "additionalProperties": false
    }
  },
  "required": ["/**"],
  "additionalProperties": false
}
```

スキーマファイルのパスは`INSERT_PATH_TO_PACKAGE/schema/`で、スキーマファイル名は`INSERT_NODE_NAME.schema.json`です。テンプレートをROSノードに適合させるには、各`INSERT_...`を置き換え、すべてのパラメーター`1..N`を追加します。
例を参照します: _Lidar Apollo Segmentation TVM Nodes_ [スキーマ](https://github.com/autowarefoundation/autoware.universe/blob/main/perception/lidar_apollo_segmentation_tvm_nodes/schema/lidar_apollo_segmentation_tvm_nodes.schema.json)

### 属性

パラメータにはいくつかの属性があり、必須のものとオプションのものがあります。オプションの属性は、パラメーターに関する有用な情報を提供し、パラメーターの値がその範囲内にあることを保証できるため、該当する場合は使用することを強くお勧めします。

#### 必須

- 名前
- 型
  - [JSONスキーマの型](http://json-schema.org/understanding-json-schema/reference/type.html)を参照してください
- 説明

#### オプション

- デフォルト
  - テストおよび検証された値。[JSONスキーマのデフォルト](https://json-schema.org/understanding-json-schema/reference/generic.html)を参照してください。
- 境界
  - 型に依存します。例えば[整数](https://json-schema.org/understanding-json-schema/reference/numeric.html#integer), [範囲](https://json-schema.org/understanding-json-schema/reference/numeric.html#range) および [サイズ](https://json-schema.org/understanding-json-schema/reference/object.html#size)

## ヒントとコツ

確立された標準を使用することで、従来のツールを使用できるようになります。以下は、VS Codeを使用してスキーマをパラメータファイルにリンクする方法の例です。これにより、開発者はオートコンプリートやパラメーターバインドされた検証などの便利な機能を利用できるようになります。

プロジェクトがホストされているルートディレクトリに、以下の2つのファイルを含む`.vscode`フォルダーを作成します。`extensions.json`の内容は以下の通りです。

```json
{
  "recommendations": ["redhat.vscode-yaml"]
}
```

また`settings.json`の内容は以下の通りです。

```json
{
  "yaml.schemas": {
    "./INSERT_PATH_TO_PACKAGE/schema/INSERT_NODE_NAME.schema.json": "**/INSERT_NODE_NAME/config/*.param.yaml"
  }
}
```

RedHat YAML拡張機能によりJSONスキーマを使用したYAMLファイルの検証が可能になり`"yaml.schemas"`設定により、`*.schema.json`ファイルが`config/`フォルダー内のすべての`*.param.yaml`ファイルに関連付けられます。
