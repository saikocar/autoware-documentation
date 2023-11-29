# ディレクトリ構造

!!! 警告

    構築中

## C++パッケージ

```txt
<package_name>
├─ config
│   ├─ foo_ros.param.yaml
│   └─ foo_non_ros.yaml
├─ doc
│   ├─ foo_document.md
│   └─ foo_diagram.svg
├─ include
│   └─ <package_name>
│       └─ foo_public.hpp
├─ launch
│   ├─ foo.launch.xml
│   └─ foo.launch.py
├─ schema
│   └─ foo_node.schema.json
├─ src
│   ├─ foo_node.cpp
│   ├─ foo_node.hpp
│   └─ foo_private.hpp
├─ test
│   └─ test_foo.cpp
├─ package.xml
├─ CMakeLists.txt
└─ README.md
```

### ディレクトリの説明

#### `config`

ノードパラメータなどの設定ファイルを配置します。
ROSパラメーターの場合は拡張子`.param.yaml`を使用します。
ROS以外のパラメーターの場合は拡張子`.yaml`を使用します。

理論的根拠: ROSパラメーターファイルは型に依存するため一部のコードフォーマッタやリンターのターゲットにするべきではありません。ファイルの種類を区別するために異なるファイル拡張子を使用します。

#### `doc`

ドキュメントファイルを配置し、READMEからリンクします。

#### `include`

ヘッダーファイルを他のパッケージに公開して配置します。`include`ディレクトリ直下にはファイルを配置せず、パッケージ名のディレクトリ下にファイルを配置してください。
このディレクトリは主にライブラリヘッダーに使用されます。多くのヘッダーをここに配置する必要はないことに注意してください。ヘッダーを`src`ディレクトリの下に配置するだけで十分です。

参考: <https://docs.ros.org/en/rolling/How-To-Guides/Ament-CMake-Documentation.html#adding-files-and-headers>

#### `launch`

launchファイル(`.launch.xml` and `.launch.py`)を配置します。

#### `schema`

パラメータ定義ファイルを配置します。詳細は[パラメータ](./parameters.md)を参照してください。

#### `src`

ソースファイルとプライベートヘッダーファイルを配置します。

#### `test`

テスト用のソースファイルを配置します。詳細については[単体テスト](../../testing-guidelines/unit-testing.md)を参照してください。

## Pythonパッケージ

未決定
