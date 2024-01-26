# マップ起動ファイル

## 概要

[Autoware の起動](../index.md)ページで説明したように、
Autoware マップ スタックは`autoware_launch.xml`で起動を開始します。
`autoware_launch`パッケージには、 `autoware_launch.xml`からマップ起動ファイルの呼び出しを開始するための
`tier4_map_component.launch.xml`が含まれています。
この図は、`autoware_launch`のAutoware マップ起動ファイル フローとautoware.universe`パッケージの一部を説明しています。

<figure markdown>
  ![map-launch-flow](images/map_launch_flow.svg){ align=center }
  <figcaption>
    Autoware マップ起動フロー図
  </figcaption>
</figure>

!!! 注記

    Autoware プロジェクトは大規模なプロジェクトです。
    したがって、Autoware プロジェクトを管理する際には、
    起動ファイル内の特定の引数を利用します。
    ROS 2 は、これらの起動ファイルの引数をオーバーライドする機能を提供します。
    詳細については[公式 ROS 2 起動ドキュメント](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.html#parameter-overrides)を参照してください。
    たとえば、
    トップレベルの起動で引数を定義すると、
    下位レベルの起動ではその値がオーバーライドされます。

tier4_map_launch パッケージの map.launch.py​​ 起動ファイルには、
マッピングに必要なノード定義が直接含まれています。Autoware の現在の設計では、`lanelet2_map_loader`,
`lanelet2_map_visualization`, `pointcloud_map_loader`、 `vector_map_tf_generator`コンポーザブル ノードは
`map_container`に含まれています。

マップ起動ファイルには多くの変更オプションがありません
(パラメーターは構成ファイルに含まれているため)。
ただし、起動時に pointcloud および LANELET2 マップの名前を指定できます
 (デフォルト値は pointcloud_map.pcd および Lanlet2_map.osm)。
たとえば、マップ ファイル名を変更する場合は、
次のコマンド ライン引数を使用して Autoware を実行できます:

```bash
ros2 launch autoware_launch autoware.launch.xml ... pointcloud_map_file:=<YOUR-PCD-FILE-NAME> lanelet2_map_file:=<YOUR-LANELET2-MAP-NAME> ...
```

または、`autoware.launch.xml`起動ファイルで変更することもできます:

```diff
- <arg name="lanelet2_map_file" default="lanelet2_map.osm" description="lanelet2 map file name"/>
+ <arg name="lanelet2_map_file" default="<YOUR-LANELET2-MAP-NAME>" description="lanelet2 map file name"/>
- <arg name="pointcloud_map_file" default="pointcloud_map.pcd" description="pointcloud map file name"/>
+ <arg name="pointcloud_map_file" default="<YOUR-PCD-FILE-NAME>" description="pointcloud map file name"/>
```
