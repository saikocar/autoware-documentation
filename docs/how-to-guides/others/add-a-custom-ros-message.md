カスタム ROS メッセージを追加する
概要
Autoware の開発中は、おそらく独自のメッセージを定義する必要があります。カスタム メッセージを追加する前に、次の手順をお読みください。

autoware_msgsのメッセージはのインターフェイスを定義しますAutoware Core。

寄稿者が に変更を加えたり、新しいメッセージを追加したい場合は、まず、 「デザイン」カテゴリautoware_msgsの下に新しいディスカッション投稿を作成する必要があります。
コンポーネント内の内部通信 (計画など) に使用されるその他のマイナー メッセージや提案メッセージは、別のリポジトリで定義する必要があります。

tier4_autoware_msgsはその一例です。
以下は、メッセージ パッケージを に追加する簡単なチュートリアルですautoware_msgs。一般的な ROS 2 チュートリアルについては、「カスタム msg および srv ファイルの作成」を参照してください。

カスタムメッセージの作成方法
Autoware ワークスペースにいることを確認し、次のコマンドを実行して新しいパッケージを作成します。例として、センサー メッセージを定義するパッケージを作成してみましょう。

パッケージを作成する

cd ./src/core/autoware_msgs
ros2 pkg create --build-type ament_cmake autoware_sensing_msgs
カスタムメッセージを作成する

.msgファイルを作成してディレクトリに配置する必要がありますmsg。

注.msg:およびファイルの頭文字は.srv大文字にする必要があります。

例として、.msgファイルを作成しGnssInsOrientation.msgてGnssInsOrientationStamped.msgGNSS/INS 方向メッセージを定義してみましょう。

mkdir msg
cd msg
touch GnssInsOrientation.msg
touch GnssInsOrientationStamped.msg
GnssInsOrientation.msgエディタを使用して次の内容になるように編集します。

geometry_msgs/Quaternion orientation
float32 rmse_rotation_x
float32 rmse_rotation_y
float32 rmse_rotation_z
この場合、カスタム メッセージは別のメッセージ パッケージのメッセージを使用しますgeometry_msgs/Quaternion。

GnssInsOrientationStamped.msgエディタを使用して次の内容になるように編集します。

std_msgs/Header header
GnssInsOrientation orientation
この場合、カスタム メッセージは別のメッセージ パッケージのメッセージを使用しますstd_msgs/Header。

CMakeLists.txt を編集する

C++このカスタム メッセージをまたは言語で使用するにはPython、次の行を に追加する必要がありますCMakeList.txt。

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/GnssInsOrientation.msg"
  "msg/GnssInsOrientationStamped.msg"
  DEPENDENCIES
    geometry_msgs
    std_msgs
  ADD_LINTER_TESTS
)
💬 このament_cmake_autoツールは非常に便利で、Autoware でより広く使用されているため、ament_cmake_autoの代わりに使用することをお勧めしますament_cmake。

交換する必要があります

find_package(ament_cmake REQUIRED)

ament_package()
と

find_package(ament_cmake_auto REQUIRED)

ament_auto_package()
package.xmlを編集する

関連する依存関係を で宣言する必要がありますpackage.xml。上記の例では、次のコンテンツを追加する必要があります。

<buildtool_depend>rosidl_default_generators</buildtool_depend>

<exec_depend>rosidl_default_runtime</exec_depend>

<depend>geometry_msgs</depend>
<depend>std_msgs</depend>

<member_of_group>rosidl_interface_packages</member_of_group>
ファイル内の<buildtool_depend>ament_cmake</buildtool_depend>を に置き換える必要があります。<buildtool_depend>ament_cmake_auto</buildtool_depend>package.xml

カスタムメッセージパッケージを構築する

たとえば次のコマンドを実行することで、ワークスペースのルートにパッケージをビルドできます。

colcon build --packages-select autoware_sensing_msgs
これで、GnssInsOrientationStampedメッセージは Autoware の他のパッケージで検出できるようになります。

Autoware でカスタム メッセージを使用する方法
次の手順に従って、Autoware でカスタム メッセージを使用できます。

に依存関係を追加しますpackage.xml。
例えば、<depend>autoware_sensing_msgs</depend>。
.hpp関連するメッセージのファイルをコードに 含めます。
例えば、#include <autoware_sensing_msgs/msg/gnss_ins_orientation_stamped.hpp>。
# Add a custom ROS message

## Overview

During the Autoware development, you will probably need to define your own messages. Read the following instructions before adding a custom message.

1. Message in [autoware_msgs](https://github.com/autowarefoundation/autoware_msgs) define interfaces of `Autoware Core`.

   - If a contributor wishes to make changes or add new messages to `autoware_msgs`, they should first create a new discussion post under the [Design category](https://github.com/orgs/autowarefoundation/discussions/categories/design).

2. Any other minor or proposal messages used for internal communication within a component(such as planning) should be defined in another repository.

   - [tier4_autoware_msgs](https://github.com/tier4/tier4_autoware_msgs) is an example of that.

The following is a simple tutorial of adding a message package to `autoware_msgs`. For the general ROS 2 tutorial, see [Create custom msg and srv files](http://docs.ros.org/en/galactic/Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.html).

## How to create custom message

Make sure you are in the Autoware workspace, and then run the following command to create a new package.
As an example, let's create a package to define sensor messages.

1. Create a package

   ```console
   cd ./src/core/autoware_msgs
   ros2 pkg create --build-type ament_cmake autoware_sensing_msgs
   ```

2. Create custom messages

   You should create `.msg` files and place them in the `msg` directory.

   **NOTE**: The initial letters of the `.msg` and `.srv` files must be capitalized.

   As an example, let's make `.msg` files `GnssInsOrientation.msg` and `GnssInsOrientationStamped.msg` to define GNSS/INS orientation messages:

   ```console
   mkdir msg
   cd msg
   touch GnssInsOrientation.msg
   touch GnssInsOrientationStamped.msg
   ```

   Edit `GnssInsOrientation.msg` with your editor to be the following content:

   ```c++
   geometry_msgs/Quaternion orientation
   float32 rmse_rotation_x
   float32 rmse_rotation_y
   float32 rmse_rotation_z
   ```

   In this case, the custom message uses a message from another message package `geometry_msgs/Quaternion`.

   Edit `GnssInsOrientationStamped.msg` with your editor to be the following content:

   ```c++
   std_msgs/Header header
   GnssInsOrientation orientation
   ```

   In this case, the custom message uses a message from another message package `std_msgs/Header`.

3. Edit CMakeLists.txt

   In order to use this custom message in `C++` or `Python` languages, we need to add the following lines to `CMakeList.txt`:

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

   :speech_balloon: The `ament_cmake_auto` tool is very useful and is more widely used in Autoware, so we recommend using `ament_cmake_auto` instead of `ament_cmake`.

   We need to replace

   ```cmake
   find_package(ament_cmake REQUIRED)

   ament_package()
   ```

   with

   ```cmake
   find_package(ament_cmake_auto REQUIRED)

   ament_auto_package()
   ```

4. Edit package.xml

   We need to declare relevant dependencies in `package.xml`. For the above example we need to add the following content:

   ```xml
   <buildtool_depend>rosidl_default_generators</buildtool_depend>

   <exec_depend>rosidl_default_runtime</exec_depend>

   <depend>geometry_msgs</depend>
   <depend>std_msgs</depend>

   <member_of_group>rosidl_interface_packages</member_of_group>
   ```

   We need to replace `<buildtool_depend>ament_cmake</buildtool_depend>` with `<buildtool_depend>ament_cmake_auto</buildtool_depend>` in the `package.xml` file.

5. Build the custom message package

   You can build the package in the root of your workspace, for example by running the following command:

   ```console
   colcon build --packages-select autoware_sensing_msgs
   ```

   Now the `GnssInsOrientationStamped` message will be discoverable by other packages in Autoware.

## How to use custom messages in Autoware

You can use the custom messages in Autoware by following these steps:

- Add dependency in `package.xml`.
  - For example, `<depend>autoware_sensing_msgs</depend>`.
- Include the `.hpp` file of the relevant message in the code.
  - For example, `#include <autoware_sensing_msgs/msg/gnss_ins_orientation_stamped.hpp>`.
