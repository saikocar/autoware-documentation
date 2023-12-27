# カスタムROSメッセージを追加する

## 概要

Autowareの開発中は、おそらく独自のメッセージを定義する必要があります。カスタムメッセージを追加する前に、次の手順をお読みください。

1. [autoware_msgs](https://github.com/autowarefoundation/autoware_msgs)のメッセージは`Autoware Core`のインターフェイスを定義します。

   - 寄稿者が`autoware_msgs`に変更を加えたり、新しいメッセージを追加したい場合は、まず[設計カテゴリ](https://github.com/orgs/autowarefoundation/discussions/categories/design)の下に新しいディスカッション投稿を作成する必要があります。

2. コンポーネント内の内部通信 (計画など) に使用されるその他のマイナーメッセージや提案メッセージは、別のリポジトリで定義する必要があります。

   - [tier4_autoware_msgs](https://github.com/tier4/tier4_autoware_msgs)はその一例です。

以下は、メッセージパッケージを`autoware_msgs`に追加する簡単なチュートリアルです。一般的な ROS 2 チュートリアルについては[カスタムmsgおよびsrvファイルの作成](http://docs.ros.org/en/galactic/Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.html)を参照してください。

## カスタムメッセージの作成方法

cd ./src/core/autoware_msgs
ros2 pkg create --build-type ament_cmake autoware_sensing_msgs
Autowareワークスペースにいることを確認し、次のコマンドを実行して新しいパッケージを作成します。
例として、センサーメッセージを定義するパッケージを作成してみましょう。

1. パッケージを作成する

   ```console
   cd ./src/core/autoware_msgs
   ros2 pkg create --build-type ament_cmake autoware_sensing_msgs
   ```

2. カスタムメッセージを作成する

   `.msg`ファイルを作成して`msg`ディレクトリに配置する必要があります。

   **注記**: `.msg`と`.srv`ファイルの頭文字は大文字にする必要があります。

   例として、`GnssInsOrientation.msg`と`GnssInsOrientationStamped.msg`の`.msg` ファイルを作成してGNSS/INS方向メッセージを定義してみましょう。:

   ```console
   mkdir msg
   cd msg
   touch GnssInsOrientation.msg
   touch GnssInsOrientationStamped.msg
   ```

   エディタを使用して`GnssInsOrientation.msg`を次の内容になるように編集します:

   ```c++
   geometry_msgs/Quaternion orientation
   float32 rmse_rotation_x
   float32 rmse_rotation_y
   float32 rmse_rotation_z
   ```

   この場合、カスタムメッセージは`geometry_msgs/Quaternion`とは別のメッセージパッケージのメッセージを使用します。

   エディタを使用して`GnssInsOrientationStamped.msg`を次の内容になるように編集します:

   ```c++
   std_msgs/Header header
   GnssInsOrientation orientation
   ```

   この場合、カスタムメッセージはstd_msgs/Header`とは別のメッセージパッケージのメッセージを使用します。

3. CMakeLists.txtを編集する

   `C++`または`Python`言語でこのカスタム メッセージを使用するには、次の行を`CMakeList.txt`に追加する必要があります:

   ```cmake
   rosidl_generate_interfaces(${PROJECT_NAME}
     "msg/GnssInsOrientation.msg"
     "msg/GnssInsOrientationStamped.msg"
     DEPENDENCIES
       geometry_msgs
       std_msgs
     ADD_LINTER_TESTS
   )
   ```

   :speech_balloon: この`ament_cmake_auto`ツールは非常に便利で、Autowareでより広く使用されているため、`ament_cmake`の代わりに使用することをお勧めします。

   交換する必要があります

   ```cmake
   find_package(ament_cmake REQUIRED)

   ament_package()
   ```

   と

   ```cmake
   find_package(ament_cmake_auto REQUIRED)

   ament_auto_package()
   ```

4. package.xmlを編集する

   関連する依存関係を`package.xml`で宣言する必要がありますpackage.xml。上記の例では、次のコンテンツを追加する必要があります:

   ```xml
   <buildtool_depend>rosidl_default_generators</buildtool_depend>

   <exec_depend>rosidl_default_runtime</exec_depend>

   <depend>geometry_msgs</depend>
   <depend>std_msgs</depend>

   <member_of_group>rosidl_interface_packages</member_of_group>
   ```

   `package.xml`ファイル内の`<buildtool_depend>ament_cmake</buildtool_depend>`を`<buildtool_depend>ament_cmake_auto</buildtool_depend>`に置き換える必要があります。

5. カスタムメッセージパッケージを構築する

   たとえば以下のコマンドを実行することで、ワークスペースのルートにパッケージをビルドできます:

   ```console
   colcon build --packages-select autoware_sensing_msgs
   ```

   これで、`GnssInsOrientationStamped`メッセージはAutowareの他のパッケージで検出できるようになります。

## Autowareでカスタムメッセージを使用する方法

次の手順に従って、Autowareでカスタムメッセージを使用できます:

- `package.xml`に依存関係を追加します。
  - 例えば、`<depend>autoware_sensing_msgs</depend>`
- 関連するメッセージの`.hpp`ファイルをコードに含めます。
  - 例えば、`#include <autoware_sensing_msgs/msg/gnss_ins_orientation_stamped.hpp>`
