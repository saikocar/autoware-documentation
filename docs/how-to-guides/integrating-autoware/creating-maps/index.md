マップの作成
Autoware には、車両の動作環境用の点群マップとベクトル マップが必要です。(詳細な仕様については、マップ設計ドキュメントのページを確認してください)。

このページでは、ユーザーが Autoware で使用できるマップを作成する方法について説明します。

点群マップの作成
従来、高精度の大規模点群マップを作成するには、モバイル マッピング システム (MMS) が使用されてきました。ただし、MMS は正確な位置決めのためにハイエンドのセンサーを必要とするため、運用コストが非常に高価になる可能性があり、比較的小規模な運転環境には適さない可能性があります。あるいは、Simultaneous Localization And Mapping (SLAM) アルゴリズムを使用して、記録された LiDAR スキャンから点群マップを作成することもできます。有用なオープンソース SLAM 実装の一部がこのページにリストされています。

使いやすい独自のソフトウェアをお好みの場合は、MAP IV, Inc.の完全自動マッピング ツール、MapIV Engineを試すことができます。現在、Autoware ユーザー向けに試用版ライセンスを無料で提供しています。

ベクトルマップの作成
Autoware 互換のベクター マップを作成する最も簡単な方法は、TIER IV, Inc.が提供する無料の Web ベース ツールであるVector Map Builderを使用することです。Vector Map Builder を使用すると、点群マップを参照として使用して車線を作成し、一時停止標識や信号機などの追加の規制要素を追加できます。

オープンソース ソフトウェア オプションの場合、MapToolbox は、Autoware 用の Lanelet2 マップを作成するために特別に設計されたUnity用のプラグインです。JOSM はLanelet2 マップの作成に使用できるもう 1 つのオープンソース ツールですが、マップを Autoware と互換性を持たせるには、多くの変更を手動で行う必要があることに注意してください。このプロセスは面倒で時間がかかる可能性があるため、JOSM の使用はお勧めできません。

Autoware 互換のマップ プロバイダー
HD マップを自分で作成できない場合は、代わりに次の Autoware 互換マップ プロバイダーのマッピング サービスを使用できます。

株式会社マップIV
アイサンテクノロジー株式会社
トムトム
以下の表は、各社のマッピング テクノロジーとサポートされている HD マップの種類を示しています。

会社	マッピング技術	利用可能なマップ
株式会社マップIV	スラム	点群マップとベクトルマップ
アイサンテクノロジー株式会社	MMS	点群マップとベクトルマップ
トムトム	MMS	ベクトルマップ*
!!! 注記

Maps provided by TomTom use their proprietary AutoStream format, not Lanelet2.
The open-source [AutoStreamForAutoware tool](https://github.com/tomtom-international/AutoStreamForAutoware) can be used to convert an AutoStream map to a Lanelet2 map.
However, the converter is still in its early stages and has some [known limitations](https://github.com/tomtom-international/AutoStreamForAutoware/blob/main/docs/known-issues.md).

# Creating maps

Autoware requires a pointcloud map and a vector map for the vehicle's operating environment. (Check the [map design documentation page](../../../design/autoware-architecture/map/index.md) for the detailed specification).

This page explains how users can create maps that can be used for Autoware.

## Creating a point cloud map

Traditionally, a Mobile Mapping System (MMS) is used in order to create highly accurate large-scale point cloud maps. However, since a MMS requires high-end sensors for precise positioning, its operational cost can be very expensive and may not be suitable for a relatively small driving environment. Alternatively, a Simultaneous Localization And Mapping (SLAM) algorithm can be used to create a point cloud map from recorded LiDAR scans. Some of the useful open-source SLAM implementations are listed in this [page](open-source-slam/index.md).

If you prefer proprietary software that is easy to use, you can try a fully automatic mapping tool from [MAP IV, Inc.](https://www.map4.jp/), [_MapIV Engine_](https://www.map4.jp/solutions/mapping-localization/map4-engine/). They currently provide a trial license for Autoware users free of charge.

## Creating a vector map

The easiest way to create an Autoware-compatible vector map is to use [Vector Map Builder](https://tools.tier4.jp/feature/vector_map_builder_ll2/), a free web-based tool provided by [TIER IV, Inc.](https://www.tier4.jp/).
Vector Map Builder allows you to create lanes and add additional regulatory elements such as stop signs or traffic lights using a point cloud map as a reference.

For open-source software options, [MapToolbox](https://github.com/autocore-ai/MapToolbox) is a plugin for [Unity](https://unity.com/) specifically designed to create Lanelet2 maps for Autoware.
Although [JOSM](https://josm.openstreetmap.de/) is another open-source tool that can be used to create Lanelet2 maps, be aware that a number of modifications must be done manually to make the map compatible with Autoware. This process can be tedious and time-consuming, so the use of JOSM is not recommended.

## Autoware-compatible map providers

If it is not possible to create HD maps yourself, you can use a mapping service from the following Autoware-compatible map providers instead:

- [MAP IV, Inc.](https://www.map4.jp/)
- [AISAN TECHNOLOGY CO., LTD.](https://www.aisantec.co.jp/)
- [TomTom](https://www.tomtom.com/)

The table below shows each company's mapping technology and the types of HD maps they support.

| **Company**                                               | **Mapping technology** | **Available maps**          |
| --------------------------------------------------------- | ---------------------- | --------------------------- |
| [MAP IV, Inc.](https://www.map4.jp/)                      | SLAM                   | Point cloud and vector maps |
| [AISAN TECHNOLOGY CO., LTD.](https://www.aisantec.co.jp/) | MMS                    | Point cloud and vector maps |
| [TomTom](https://www.tomtom.com/)                         | MMS                    | Vector map\*                |

!!! note

    Maps provided by TomTom use their proprietary AutoStream format, not Lanelet2.
    The open-source [AutoStreamForAutoware tool](https://github.com/tomtom-international/AutoStreamForAutoware) can be used to convert an AutoStream map to a Lanelet2 map.
    However, the converter is still in its early stages and has some [known limitations](https://github.com/tomtom-international/AutoStreamForAutoware/blob/main/docs/known-issues.md).
