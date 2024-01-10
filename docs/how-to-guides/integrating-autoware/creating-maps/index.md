# マップの作成

Autoware には、車両の動作環境用の点群マップとベクターマップが必要です。(詳細な仕様については、[マップ設計ドキュメントのページ](../../../design/autoware-architecture/map/index.md)を確認してください)。

このページでは、ユーザーがAutowareで使用できるマップを作成する方法について説明します。

## 点群マップの作成

従来、高精度の大規模点群マップを作成するには、モバイルマッピングシステム (MMS) が使用されてきました。ただし、MMSは正確な位置決めのためにハイエンドのセンサーを必要とするため、運用コストが非常に高価になる可能性があり、比較的小規模な運転環境には適さない可能性があります。あるいは、Simultaneous Localization And Mapping (SLAM)アルゴリズムを使用して、記録された LiDAR キャンから点群マップを作成することもできます。有用なオープンソース SLAM実装の一部がこの[ページ](open-source-slam/index.md)にリストされています。

使いやすい独自のソフトウェアをお好みの場合は、[MAP IV, Inc.](https://www.map4.jp/)の完全自動マッピング ツール、[_MapIV Engine_](https://www.map4.jp/solutions/mapping-localization/map4-engine/)を試すことができます。現在、Autowareユーザー向けに試用版ライセンスを無料で提供しています。

## ベクターマップの作成

Autoware 互換のベクター マップを作成する最も簡単な方法は、[TIER IV, Inc.](https://www.tier4.jp/)が提供する無料の Web ベース ツールである[Vector Map Builder](https://tools.tier4.jp/feature/vector_map_builder_ll2/)を使用することです。
Vector Map Builder を使用すると、点群マップを参照として使用して車線を作成し、一時停止標識や信号機などの追加の規制要素を追加できます。

オープンソースソフトウェアオプションの場合、[MapToolbox](https://github.com/autocore-ai/MapToolbox)は、Autoware用のLanelet2マップを作成するために特別に設計された[Unity](https://unity.com/)用のプラグインです。
[JOSM](https://josm.openstreetmap.de/)はLanelet2マップの作成に使用できるもう1つのオープンソースツールですが、マップをAutowareと互換性を持たせるには、多くの変更を手動で行う必要があることに注意してください。このプロセスは面倒で時間がかかる可能性があるため、JOSM の使用はお勧めできません。

## Autoware互換のマップ プロバイダー

HDマップを自分で作成できない場合は、代わりに次のAutoware互換マッププロバイダーのマッピングサービスを使用できます:

- [MAP IV, Inc.](https://www.map4.jp/)
- [AISAN TECHNOLOGY CO., LTD.](https://www.aisantec.co.jp/)
- [TomTom](https://www.tomtom.com/)

以下の表は、各社のマッピングテクノロジーとサポートされているHDマップの種類を示しています。

| **会社**                                               | **マッピング技術** | **利用可能なマップ**          |
| --------------------------------------------------------- | ---------------------- | --------------------------- |
| [MAP IV, Inc.](https://www.map4.jp/)                      | SLAM                   | 点群マップとベクターマップ |
| [AISAN TECHNOLOGY CO., LTD.](https://www.aisantec.co.jp/) | MMS                    | 点群マップとベクターマップ |
| [TomTom](https://www.tomtom.com/)                         | MMS                    | ベクターマップ\*                |

!!! 注記

    TomTom が提供するマップは、Lanelet2ではなく、独自のAutoStream形式を使用します。
    オープンソースの [AutoStreamForAutoware tool](https://github.com/tomtom-international/AutoStreamForAutoware)を使用して、AutoStream マップを Lanelet2 マップに変換できます。
    ただし、コンバーターはまだ初期段階にあり、いくつかの[既知の制限](https://github.com/tomtom-international/AutoStreamForAutoware/blob/main/docs/known-issues.md)があります。
