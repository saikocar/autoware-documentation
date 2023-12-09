# 地図コンポーネントの設計

## 1. 概要

Autowareは、運転環境の高解像度の点群マップとベクターマップを利用して、位置推定、経路計画、信号機の検出、歩行者や他の車両の軌跡の予測などのさまざまなタスクを実行します。

このドキュメントでは、Autowareの地図コンポーネントの設計について説明します。これには要件、アーキテクチャ設計、機能、データ形式、地図情報を残りの自動運転スタックに配信するためのインターフェイスが含まれます。

## 2. 要件

地図はスタックの残りの部分に 2種類の情報を提供する必要があります:

- ベクターマップとしての道路に関する意味情報
- 点群マップとしての環境に関する幾何学的情報(オプション)

ベクターマップには、道路網、車線の形状、信号機に関する高精度の情報が含まれています。ルート計画、信号機の検出、他の車両や歩行者の軌跡の予測に必要です。

3D点群マップは、主にLiDARベースの位置特定とAutowareの認識の一部に使用されます。車両の現在の位置と方向を決定するために、1つ以上のLiDAR ユニットからキャプチャされたライブ スキャンが、事前に生成された 3D 点群マップと照合されます。したがって、良好なローカリゼーション結果を得るには、正確な点群マップが不可欠です。ただし、車両に十分な精度を備えた代替位置推定方法 (たとえば、カメラベースの位置推定を使用する場合) がある場合は、Autoware を使用するために点群マップが必要ない場合があります。

上記の 2 種類のマップに加えて、Autoware ではマップの座標系を測地系で指定するための補足ファイルも必要です。

## 3. アーキテクチャ

点群マップの読み込み: 点群マップを読み込み、公開します。
ベクター マップの読み込み: ベクター マップを読み込み、公開します。
投影読み込み: ローカル座標 (x、y、z) と測地座標 (緯度、経度、高度) の間の変換のための投影情報を読み込み、公開します。

この図はAutowareの地図コンポーネントの高レベルのアーキテクチャを示しています。

![map component architecture](image/high-level-map-diagram.drawio.svg){width="800"}

Map コンポーネントは次のサブコンポーネントで構成されます:

- **Point Cloud Map Loading**: 点群地図を読み込み、配信します。
- **Vector Map Loading**: ベクターマップを読み込み、配信します。
- **Projection Loading**: ローカル座標(x, y, z)と測地座標 (緯度、経度、高度) の間の変換のための投影情報を読み込み、配信します。

## 4. コンポーネントインターフェース

### 地図コンポーネントへの入力

- **ファイルシステムから**
  - 点群マップとそのメタデータファイル
  - ベクターマップ
  - 投影情報

### 地図コンポーネントからの出力

- **計測へ**
  - 投影情報: GNSSデータを測地座標系からローカル座標系に変換するために使用されます。
- **位置推定へ**
  - 点群地図: LiDARベースの位置推定に使用されます。
  - ベクターマップ: 道路標示などに基づく位置特定方法に使用されます。
- **知覚へ**
  - 点群地図: LiDARと点群マップを比較することで障害物の検出に使用されます。
  - ベクターマップ: 車両軌道予測に使用
- **計画へ**
  - ベクターマップ: 行動計画に使用されます。
- **APIレイヤーへ**
  - 投影情報: 位置特定結果をローカル座標系から測地座標系に変換するために使用されます。

## 5. 地図仕様

### 点群地図

点群地図は、次の要件を満たすファイルとして提供する必要があります:

- 点群マップは、lanelet2マップおよびローカル座標と測地座標の間で変換する他のパッケージとの一貫性を保つために、`map_projection_loader`で定義された同じ座標に投影する必要があります。詳細については[`map_projection_loader`のreadme](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_projection_loader/README.md)を参照してください。
- [PCD (点群データ)ファイル形式](https://pointclouds.org/documentation/tutorials/pcd_file_format.html)ファイル形式である必要がありますが、単一のPCDファイルまたは複数の PCD ファイルに分割することができます。
- マップ内の各ポイントには、X、Y、Z 座標が含まれている必要があります。
- 各ポイントの強度またはRGB値をオプションで含めることができます。
- 車両の動作領域全体をカバーする必要があります。また、車両に取り付けられたセンサーの検出範囲に応じて、追加の緩衝ゾーンを含めることをお勧めします。
- 信頼性の高い位置特定結果を得るには、解像度が少なくとも0.2mである必要があります。
- ローカル座標でもグローバル座標でも構いませんが、位置特定にGNSSデータを使用するにはグローバル座標(地理参照)である必要があります。

分割マップ形式の詳細については[`map_loader` in Autoware Universeの`map_loader`のreadme](https://github.com/autowarefoundation/autoware.universe/blob/main/map/map_loader/README.md)を参照してください

!!! 注記

    Autowareでは現在[Military Grid Reference System (MGRS)](https://en.wikipedia.org/wiki/Military_Grid_Reference_System), [Universal Transverse Mercator (UTM)](https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system)および[Japan Rectangular Coordinate System](https://ja.wikipedia.org/wiki/%E5%B9%B3%E9%9D%A2%E7%9B%B4%E8%A7%92%E5%BA%A7%E6%A8%99%E7%B3%BB)の3つのグローバル座標系がサポートされています。
    ただし、MGRSは地理参照マップに推奨される座標系です。
    MGRS座標系のマップでは、各ポイントのX座標とY座標は100,000メートル四方内のポイントの位置を表し、Z座標はポイントの標高を表します。

### ベクターマップ

ベクター点群マップは以下の要件を満たすファイルとして提供する必要があります:

- これは[Lanelet2](https://github.com/fzi-forschungszentrum-informatik/Lanelet2)形式である必要があり、[additional modifications required by Autowareによる追加の変更が必要です](https://github.com/autowarefoundation/autoware_common/blob/main/tmp/lanelet2_extension/docs/lanelet2_format_extension.md)。
- 車線、信号機、停止線、横断歩道、駐車スペース、駐車場の形状と位置情報が含まれている必要があります。
- 道路の始点または終点を除き、マップ内の各小道は、その前、後続、左隣、および右隣に正しく接続されている必要があります。
- 地図内の各レーンレットには、制限速度、通行権、交通方向、関連する信号機、停止線、交通標識などの交通ルール情報が含まれている必要があります。
- 車両の動作領域全体をカバーする必要があります。

!!! 警告

    構築中

### 投影情報

詳細については、Autoware Universeの Readmemap_projection_loaderを参照してください。

投影情報は、次の要件を満たすファイルとして提供する必要があります:

- 現在のAutoware Universeの実装で提供される`map_projection_loader`へはYAML形式である必要があります。
- ファイルには次の情報が含まれている必要があります:
  - ローカル座標とグローバル座標の間の変換に使用される投影法の名前
  - 投影法のパラメータ（投影法に応じて）

詳細については[`map_projection_loader` in Autoware Universeの`map_projection_loader`のreadme](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_projection_loader/README.md)を参照してください。
