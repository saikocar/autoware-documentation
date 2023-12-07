知覚起動ファイル
概要
「Autoware の起動」autoware_launch.xmlページで説明したように、Autoware 認識スタックは で起動を開始します。パッケージに は、認識起動ファイルの呼び出しを開始するための が含まれています。この図は、パッケージでの Autoware 認識起動ファイル フローの一部を説明しています。autoware_launchtier4_perception_component.launch.xmlautoware_launch.xmlautoware_launchautoware.universe

![perception-launch-flow](images/perception_launch_flow.svg){ align=center } Autoware 認識起動フロー図
!!! 注記

The Autoware project is a large project.
Therefore, as we manage the Autoware project, we utilize specific
arguments in the launch files.
ROS 2 offers an argument-overriding feature for these launch files.
Please refer to [the official ROS 2 launch documentation](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.html#parameter-overrides) for further information.
For instance,
if we define an argument at the top-level launch,
it will override the value on lower-level launches.
tier4_perception_component.launch.xml
起動tier4_perception_component.launch.xmlファイルは、パッケージで起動される主要な認識コンポーネントですautoware_launch。この起動ファイルは、リポジトリからtier4_perception_launchパッケージperception.launch.xmlを呼び出します。tier4_perception_component.launch.xml で知覚起動引数を変更できます。また、これは他の知覚起動ファイルの最上位の起動ファイルであるため、変更したい他の必要な引数を追加することもできます。以下に、事前に定義された認識起動引数をいくつか示します。autoware.universetier4_perception_component.launch.xml

occupancy_grid_map_method:この引数は、知覚スタックの占有グリッド マップ方法を決定します。詳細については、probabilistic_occupancy_grid_mapパッケージを確認してください。デフォルトの確率的占有グリッド マップ方法は ですpointcloud_based_occupancy_grid_map。これを に変更したい場合はlaserscan_based_occupancy_grid_map、ここで変更できます。

- <arg name="occupancy_grid_map_method" default="pointcloud_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
+ <arg name="occupancy_grid_map_method" default="laserscan_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
detected_objects_filter_method:この引数は、検出されたオブジェクトのフィルター方法を決定します。レーンレットと位置フィルターの詳細については、detected_object_validationパッケージを確認してください。デフォルトの検出オブジェクト フィルター方法は ですlanelet_filter。これを に変更したい場合はposition_filter、ここで変更できます。

- <arg name="detected_objects_filter_method" default="lanelet_filter" description="options: lanelet_filter, position_filter"/>
+ <arg name="detected_objects_filter_method" default="position_filter" description="options: lanelet_filter, position_filter"/>
detected_objects_validation_method:この引数は、検出されたオブジェクトの検証方法を決定します。検証方法の詳細については、detected_object_validationパッケージを確認してください。デフォルトの検出オブジェクト フィルター方法は ですobstacle_pointcloud。これを に変更したい場合はoccupancy_grid、ここで変更できますが、次のようlaserscan_based_occupancy_grid_mapなメソッドが必要であることに注意してくださいoccupancy_grid_map_method。

- <arg name="occupancy_grid_map_method" default="pointcloud_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
+ <arg name="occupancy_grid_map_method" default="laserscan_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
  <arg
  name="detected_objects_validation_method"
- default="obstacle_pointcloud"
+ default="occupancy_grid"
  description="options: obstacle_pointcloud, occupancy_grid (occupancy_grid_map_method must be laserscan_based_occupancy_grid_map)"
  />
!!! 注記

You can also use this arguments as command line arguments:
```bash
ros2 launch autoware_launch autoware.launch.xml ... detected_objects_filter_method:=lanelet_filter occupancy_grid_map_method:=laserscan_based_occupancy_grid_map ...
```
定義済みの引数については上で説明しましたが、 tier4_perception_launchの起動ファイルtier4_perception_component.launch.xmlには多数の認識引数が含まれています。リポジトリをフォークしていないため、必要な起動引数を tier4_perception_component.launch.xml ファイルに追加できます。いくつかの例についてはガイドラインに従ってください。perception.launch.xmlautoware.universe

知覚.launch.xml
起動perception.launch.xmlファイルは、 での主要な認識起動ですautoware.universe。この起動ファイルは、上で説明したように、必要な認識起動ファイルを呼び出しますAutoware perception launch flow diagram。のトップレベルの起動ファイルはperception.launch.xmlであるtier4_perception_component.launch.xmlため、 で何かを変更したい場合は、の代わりにperception.launch.xmlこれらの変更を適用します。tier4_perception_component.launch.xmlperception.launch.xml

以下に、認識パイプラインの変更例をいくつか示します。

remove_unknown:このパラメータは、カメラとライダーの融合時に未知のオブジェクトを削除するかどうかを決定します。詳細については、roi_cluster_fusionノードを確認してください。デフォルト値は ですtrue。これを に変更したい場合はfalse、この引数を に追加すると、のtier4_perception_component.launch.xmlがオーバーライドされます。perception.launch.xmlargument

+ <arg name="remove_unknown" default="false"/>
camera topics:カメラとライダーの融合またはカメラとライダーとレーダーの融合を知覚モードとして使用している場合は、カメラと情報のトピックも追加できます。tier4_perception_component.launch.xmlこれにより、perception.launch.xml起動ファイルの引数がオーバーライドされます。

+ <arg name="image_raw0" default="/sensing/camera/camera0/image_rect_color" description="image raw topic name"/>
+ <arg name="camera_info0" default="/sensing/camera/camera0/camera_info" description="camera info topic name"/>
+ <arg name="detection_rois0" default="/perception/object_recognition/detection/rois0" description="detection rois output topic name"/>
...
これらの例のように、起動ファイルに必要なすべての引数を追加できますtier4_perception_component.launch.xml。
# Perception Launch Files

## Overview

The Autoware perception stacks start
launching at `autoware_launch.xml` as we mentioned at [Launch Autoware](../index.md) page.
The `autoware_launch` package includes `tier4_perception_component.launch.xml`
for starting perception launch files invocation from `autoware_launch.xml`.
This diagram describes some of the Autoware perception launch files flow at `autoware_launch` and `autoware.universe` packages.

<figure markdown>
  ![perception-launch-flow](images/perception_launch_flow.svg){ align=center }
  <figcaption>
    Autoware perception launch flow diagram
  </figcaption>
</figure>

!!! note

    The Autoware project is a large project.
    Therefore, as we manage the Autoware project, we utilize specific
    arguments in the launch files.
    ROS 2 offers an argument-overriding feature for these launch files.
    Please refer to [the official ROS 2 launch documentation](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.html#parameter-overrides) for further information.
    For instance,
    if we define an argument at the top-level launch,
    it will override the value on lower-level launches.

## tier4_perception_component.launch.xml

The `tier4_perception_component.launch.xml` launch file is the main perception component launch at the `autoware_launch` package.
This launch file calls `perception.launch.xml` at [tier4_perception_launch](https://github.com/autowarefoundation/autoware.universe/tree/main/launch/tier4_perception_launch) package from `autoware.universe` repository.
We can modify perception launch arguments at tier4_perception_component.launch.xml.
Also,
we can add any other necessary arguments
that we want
to change it since `tier4_perception_component.launch.xml` is the top-level launch file of other perception launch files.
Here are some predefined perception launch arguments:

- **`occupancy_grid_map_method:`** This argument determines the occupancy grid map method for perception stack. Please check [probabilistic_occupancy_grid_map](https://autowarefoundation.github.io/autoware.universe/main/perception/probabilistic_occupancy_grid_map/) package for detailed information.
  The default probabilistic occupancy grid map method is `pointcloud_based_occupancy_grid_map`.
  If you want to change it to the `laserscan_based_occupancy_grid_map`, you can change it here:

  ```diff
  - <arg name="occupancy_grid_map_method" default="pointcloud_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
  + <arg name="occupancy_grid_map_method" default="laserscan_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
  ```

- **`detected_objects_filter_method:`** This argument determines the filter method for detected objects.
  Please check [detected_object_validation](https://autowarefoundation.github.io/autoware.universe/main/perception/detected_object_validation/) package for detailed information about lanelet and position filter.
  The default detected object filter method is `lanelet_filter`.
  If you want to change it to the `position_filter`, you can change it here:

  ```diff
  - <arg name="detected_objects_filter_method" default="lanelet_filter" description="options: lanelet_filter, position_filter"/>
  + <arg name="detected_objects_filter_method" default="position_filter" description="options: lanelet_filter, position_filter"/>
  ```

- **`detected_objects_validation_method:`** This argument determines the validation method for detected objects.
  Please check [detected_object_validation](https://autowarefoundation.github.io/autoware.universe/main/perception/detected_object_validation/) package for detailed information about validation methods.
  The default detected object filter method is `obstacle_pointcloud`.
  If you want to change it to the `occupancy_grid`, you can change it here,
  but remember it requires `laserscan_based_occupancy_grid_map` method as `occupancy_grid_map_method`:

  ```diff
  - <arg name="occupancy_grid_map_method" default="pointcloud_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
  + <arg name="occupancy_grid_map_method" default="laserscan_based_occupancy_grid_map" description="options: pointcloud_based_occupancy_grid_map, laserscan_based_occupancy_grid_map"/>
    <arg
    name="detected_objects_validation_method"
  - default="obstacle_pointcloud"
  + default="occupancy_grid"
    description="options: obstacle_pointcloud, occupancy_grid (occupancy_grid_map_method must be laserscan_based_occupancy_grid_map)"
    />
  ```

!!! note

    You can also use this arguments as command line arguments:
    ```bash
    ros2 launch autoware_launch autoware.launch.xml ... detected_objects_filter_method:=lanelet_filter occupancy_grid_map_method:=laserscan_based_occupancy_grid_map ...
    ```

The predefined `tier4_perception_component.launch.xml` arguments explained above,
but there is the lot of perception arguments
included in `perception.launch.xml` launch file at [tier4_perception_launch](https://github.com/autowarefoundation/autoware.universe/tree/main/launch/tier4_perception_launch).
Since we didn't fork `autoware.universe` repository,
we can add the necessary launch argument to tier4_perception_component.launch.xml file.
Please follow the guidelines for some examples.

## perception.launch.xml

The `perception.launch.xml` launch file is the main perception launch at the `autoware.universe`.
This launch file calls necessary perception launch files
as we mentioned [`Autoware perception launch flow diagram`](#overview) above.
The top-level launch file of `perception.launch.xml` is `tier4_perception_component.launch.xml`,
so if we want to change anything on `perception.launch.xml`,
we will apply these changes `tier4_perception_component.launch.xml` instead of `perception.launch.xml`.

Here are some example changes for the perception pipeline:

- **`remove_unknown:`** This parameter determines the remove unknown objects at camera-lidar fusion.
  Please check [roi_cluster_fusion](https://github.com/autowarefoundation/autoware.universe/blob/main/perception/image_projection_based_fusion/docs/roi-cluster-fusion.md) node for detailed information.
  The default value is `true`.
  If you want to change it to the `false`,
  you can add this argument to `tier4_perception_component.launch.xml`,
  so it will override the `perception.launch.xml`'s `argument`:

  ```diff
  + <arg name="remove_unknown" default="false"/>
  ```

- **`camera topics:`** If you are using camera-lidar fusion or camera-lidar-radar fusion as a perception_mode,
  you can add your camera and info topics on `tier4_perception_component.launch.xml` as well,
  it will override the `perception.launch.xml` launch file arguments:

  ```diff
  + <arg name="image_raw0" default="/sensing/camera/camera0/image_rect_color" description="image raw topic name"/>
  + <arg name="camera_info0" default="/sensing/camera/camera0/camera_info" description="camera info topic name"/>
  + <arg name="detection_rois0" default="/perception/object_recognition/detection/rois0" description="detection rois output topic name"/>
  ...
  ```

You can add every necessary argument
to `tier4_perception_component.launch.xml` launch file like these examples.
