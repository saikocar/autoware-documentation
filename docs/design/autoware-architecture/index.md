# アーキテクチャの概要

このページでは Autoware のアーキテクチャについて説明します。

## 導入

現在のAutowareは、各モジュールの役割を明確にし、それらの間のインターフェイスを簡素化する階層化アーキテクチャとして定義されています。そうすることによって:

- Autowareの内部処理がより透明になります。
- モジュール間の相互依存性が軽減されるため、共同開発が容易になります。
- ユーザーは、Autowareのインターフェイスに合うようにソフトウェアをラップするだけで、既存のモジュール(位置推定など)を独自のソフトウェアコンポーネントに簡単に置き換えることができます。

このアーキテクチャ設計の当初の焦点は駆動能力のみにあったため、次の機能は将来の作業として残されたことに注意してください:

- フェイルセーフ
- ヒューマン・マシン・インターフェース
- リアルタイム処理
- 冗長化システム
- 状態監視システム

## 高階層のアーキテクチャ設計

![概要](image/autoware-architecture-overview.drawio.svg)

Autowareのアーキテクチャは次の6つのスタックで構成されます。リンクされた各ページには、そのスタックに固有のより詳細な要件とユースケースのセットが含まれています:

- [計測設計](sensing/index.md)
- [地図設計](map/index.md)
- [位置推定設計](localization/index.md)
- [知覚設計](perception/index.md)
- [計画設計](planning/index.md)
- [制御設計](control/index.md)
- [車両インターフェース設計](vehicle/index.md)

## ノード図

デフォルト構成のAutowareノードを示す図は、[ノード図](node-diagram/index.md)ページにあります。 各ノードの詳細なドキュメントは[Autoware Universeのドキュメント](https://autowarefoundation.github.io/autoware.universe/main/)で入手できます。

Autoware構成は拡張/選択可能であり、環境と必要なユースケースによって異なることに注意してください。

## 参考

- [AWF技術運営委員会に対して行われたアーキテクチャのプレゼンテーション、2020年3月](https://discourse.ros.org/uploads/short-url/woUU7TGLPXFCTJLtht11rJ0SqCL.pdf)
