地図コンポーネントの設計
1。概要
Autoware は、運転環境の高解像度の点群マップとベクトル マップを利用して、位置特定、ルート計画、信号機の検出、歩行者や他の車両の軌跡の予測などのさまざまなタスクを実行します。

このドキュメントでは、Autoware の地図コンポーネントの設計について説明します。これには、要件、アーキテクチャ設計、機能、データ形式、地図情報を残りの自動運転スタックに配信するためのインターフェイスが含まれます。

2. 要件
マップはスタックの残りの部分に 2 種類の情報を提供する必要があります。

ベクトル地図としての道路に関する意味情報
点群マップとしての環境に関する幾何学的情報 (オプション)
ベクター マップには、道路網、車線の形状、信号機に関する高精度の情報が含まれています。ルート計画、信号機の検出、他の車両や歩行者の軌跡の予測に必要です。

3D 点群マップは、主に LiDAR ベースの位置特定と Autoware の認識の一部に使用されます。車両の現在の位置と方向を決定するために、1 つ以上の LiDAR ユニットからキャプチャされたライブ スキャンが、事前に生成された 3D 点群マップと照合されます。したがって、良好なローカリゼーション結果を得るには、正確な点群マップが不可欠です。ただし、車両に十分な精度を備えた代替ローカリゼーション方法 (たとえば、カメラベースのローカリゼーションを使用する場合) がある場合は、Autoware を使用するために点群マップが必要ない場合があります。

上記の 2 種類のマップに加えて、Autoware ではマップの座標系を測地系で指定するための補足ファイルも必要です。

3. アーキテクチャ
この図は、Autoware の Map コンポーネントの高レベルのアーキテクチャを示しています。

マップコンポーネントのアーキテクチャ{幅="800"}

Map コンポーネントは次のサブコンポーネントで構成されます。

点群マップの読み込み: 点群マップを読み込み、公開します。
ベクター マップの読み込み: ベクター マップを読み込み、公開します。
投影読み込み: ローカル座標 (x、y、z) と測地座標 (緯度、経度、高度) の間の変換のための投影情報を読み込み、公開します。
4. コンポーネントインターフェース
マップコンポーネントへの入力
ファイルシステムから
点群マップとそのメタデータ ファイル
ベクトルマップ
投影情報
マップコンポーネントからの出力
センシングへ
投影情報: GNSS データを測地座標系からローカル座標系に変換するために使用されます。
ローカリゼーションへ
点群マップ: LiDAR ベースの位置特定に使用されます。
ベクトルマップ: 道路標示などに基づく位置特定方法に使用されます。
知覚へ
点群マップ: LiDAR と点群マップを比較することで障害物のセグメンテーションに使用されます。
ベクトルマップ：車両軌道予測に使用
企画へ
ベクトル マップ: 行動計画に使用されます。
API層へ
投影情報: 位置特定結果をローカル座標系から測地座標系に変換するために使用されます。
5. マップ仕様
点群マップ
点群マップは、次の要件を満たすファイルとして提供する必要があります。

点群マップは、map_projection_loaderlananelet2 マップおよびローカル座標と測地座標の間で変換する他のパッケージとの一貫性を保つために、 で定義された同じ座標に投影する必要があります。詳細については、の Readmemap_projection_loaderを参照してください。
PCD (点群データ) ファイル形式である必要がありますが、単一の PCD ファイルまたは複数の PCD ファイルに分割することができます。
マップ内の各ポイントには、X、Y、Z 座標が含まれている必要があります。
各ポイントの強度または RGB 値をオプションで含めることができます。
車両の動作領域全体をカバーする必要があります。また、車両に取り付けられたセンサーの検出範囲に応じて、追加の緩衝ゾーンを含めることをお勧めします。
信頼性の高い位置特定結果を得るには、解像度が少なくとも 0.2 m である必要があります。
ローカル座標でもグローバル座標でも構いませんが、位置特定に GNSS データを使用するにはグローバル座標 (地理参照) である必要があります。
分割マップ形式の詳細については、Autoware Universeの Readmemap_loaderを参照してください。

!!! 注記

Three global coordinate systems are currently supported by Autoware, including [Military Grid Reference System (MGRS)](https://en.wikipedia.org/wiki/Military_Grid_Reference_System), [Universal Transverse Mercator (UTM)](https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system), and [Japan Rectangular Coordinate System](https://ja.wikipedia.org/wiki/%E5%B9%B3%E9%9D%A2%E7%9B%B4%E8%A7%92%E5%BA%A7%E6%A8%99%E7%B3%BB).
However, MGRS is a preferred coordinate system for georeferenced maps.
In a map with MGRS coordinate system, the X and Y coordinates of each point represent the point's location within the 100,000-meter square, while the Z coordinate represents the point's elevation.
ベクトルマップ
ベクトル クラウド マップは、次の要件を満たすファイルとして提供する必要があります。

これはLanelet2形式である必要があり、Autoware による追加の変更が必要です。
車線、信号機、停止線、横断歩道、駐車スペース、駐車場の形状と位置情報が含まれている必要があります。
道路の始点または終点を除き、マップ内の各小道は、その前、後続、左隣、および右隣に正しく接続されている必要があります。
地図内の各レーンレットには、制限速度、通行権、交通方向、関連する信号機、停止線、交通標識などの交通ルール情報が含まれている必要があります。
車両の動作領域全体をカバーする必要があります。
!!! 警告

Under Construction
投影情報
投影情報は、次の要件を満たすファイルとして提供する必要があります。

現在の Autoware Universe 実装で提供される YAML 形式である必要がありますmap_projection_loader。
ファイルには次の情報が含まれている必要があります。
ローカル座標とグローバル座標の間の変換に使用される投影法の名前
投影法のパラメータ（投影法に応じて）
詳細については、Autoware Universeの Readmemap_projection_loaderを参照してください。
# Map component design

## 1. Overview

Autoware relies on high-definition point cloud maps and vector maps of the driving environment to perform various tasks such as localization, route planning, traffic light detection, and predicting the trajectories of pedestrians and other vehicles.

This document describes the design of map component of Autoware, including its requirements, architecture design, features, data formats, and interface to distribute map information to the rest of autonomous driving stack.

## 2. Requirements

Map should provide two types of information to the rest of the stack:

- Semantic information about roads as a vector map
- Geometric information about the environment as a point cloud map (optional)

A vector map contains highly accurate information about a road network, lane geometry, and traffic lights. It is required for route planning, traffic light detection, and predicting the trajectories of other vehicles and pedestrians.

A 3D point cloud map is primarily used for LiDAR-based localization and part of perception in Autoware. In order to determine the current position and orientation of the vehicle, a live scan captured from one or more LiDAR units is matched against a pre-generated 3D point cloud map. Therefore, an accurate point cloud map is crucial for good localization results. However, if the vehicle has an alternate localization method with enough accuracy, for example using camera-based localization, point cloud map may not be required to use Autoware.

In addition to above two types of maps, Autoware also requires a supplemental file for specifying the coordinate system of the map in geodetic system.

## 3. Architecture

This diagram describes the high-level architecture of Map component in Autoware.

![map component architecture](image/high-level-map-diagram.drawio.svg){width="800"}

The Map component consists of the following sub-components:

- **Point Cloud Map Loading**: Load and publish point cloud map
- **Vector Map Loading**: Load and publish vector map
- **Projection Loading**: Load and publish projection information for conversion between local coordinate (x, y, z) and geodetic coordinate (latitude, longitude, altitude)

## 4. Component interface

### Input to the map component

- **From file system**
  - Point cloud map and its metadata file
  - Vector map
  - Projection information

### Output from the map component

- **To Sensing**
  - Projection information: Used to convert GNSS data from geodetic coordinate system to local coordinate system
- **To Localization**
  - Point cloud map: Used for LiDAR-based localization
  - Vector map: Used for localization methods based on road markings, etc
- **To Perception**
  - Point cloud map: Used for obstacle segmentation by comparing LiDAR and point cloud map
  - Vector map: Used for vehicle trajectory prediction
- **To Planning**
  - Vector map: Used for behavior planning
- **To API layer**
  - Projection information: Used to convert localization results from local coordinate system to geodetic coordinate system

## 5. Map Specification

### Point Cloud Map

The point cloud map must be supplied as a file with the following requirements:

- The point cloud map must be projected on the same coordinate defined in `map_projection_loader` in order to be consistent with the lanelet2 map and other packages that converts between local and geodetic coordinates. For more information, please refer to [the readme of `map_projection_loader`](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_projection_loader/README.md).
- It must be in the [PCD (Point Cloud Data) file format](https://pointclouds.org/documentation/tutorials/pcd_file_format.html), but can be a single PCD file or divided into multiple PCD files.
- Each point in the map must contain X, Y, and Z coordinates.
- An intensity or RGB value for each point may be optionally included.
- It must cover the entire operational area of the vehicle. It is also recommended to include an additional buffer zone according to the detection range of sensors attached to the vehicle.
- Its resolution should be at least 0.2 m to yield reliable localization results.
- It can be in either local or global coordinates, but must be in global coordinates (georeferenced) to use GNSS data for localization.

For more details on divided map format, please refer to [the readme of `map_loader` in Autoware Universe](https://github.com/autowarefoundation/autoware.universe/blob/main/map/map_loader/README.md).

!!! note

    Three global coordinate systems are currently supported by Autoware, including [Military Grid Reference System (MGRS)](https://en.wikipedia.org/wiki/Military_Grid_Reference_System), [Universal Transverse Mercator (UTM)](https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system), and [Japan Rectangular Coordinate System](https://ja.wikipedia.org/wiki/%E5%B9%B3%E9%9D%A2%E7%9B%B4%E8%A7%92%E5%BA%A7%E6%A8%99%E7%B3%BB).
    However, MGRS is a preferred coordinate system for georeferenced maps.
    In a map with MGRS coordinate system, the X and Y coordinates of each point represent the point's location within the 100,000-meter square, while the Z coordinate represents the point's elevation.

### Vector Map

The vector cloud map must be supplied as a file with the following requirements:

- It must be in [Lanelet2](https://github.com/fzi-forschungszentrum-informatik/Lanelet2) format, with [additional modifications required by Autoware](https://github.com/autowarefoundation/autoware_common/blob/main/tmp/lanelet2_extension/docs/lanelet2_format_extension.md).
- It must contain the shape and position information of lanes, traffic lights, stop lines, crosswalks, parking spaces, and parking lots.
- Except at the beginning or end of a road, each lanelet in the map must be correctly connected to its predecessor, successors, left neighbor, and right neighbor.
- Each lanelet in the map must contain traffic rule information including its speed limit, right of way, traffic direction, associated traffic lights, stop lines, and traffic signs.
- It must cover the entire operational area of the vehicle.

!!! warning

    Under Construction

### Projection Information

The projection information must be supplied as a file with the following requirements:

- It must be in YAML format, provided into `map_projection_loader` in current Autoware Universe implementation.
- The file must contain the following information:
  - The name of the projection method used to convert between local and global coordinates
  - The parameters of the projection method (depending on the projection method)

For further information, please refer to [the readme of `map_projection_loader` in Autoware Universe](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_projection_loader/README.md).
