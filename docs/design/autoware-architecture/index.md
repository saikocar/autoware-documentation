アーキテクチャの概要
このページでは Autoware のアーキテクチャについて説明します。

導入
現在の Autoware は、各モジュールの役割を明確にし、それらの間のインターフェイスを簡素化する階層化アーキテクチャとして定義されています。そうすることによって：

Autoware の内部処理がより透明になります。
モジュール間の相互依存性が軽減されるため、共同開発が容易になります。
ユーザーは、Autoware のインターフェイスに合うようにソフトウェアをラップするだけで、既存のモジュール (ローカリゼーションなど) を独自のソフトウェア コンポーネントに簡単に置き換えることができます。
このアーキテクチャ設計の当初の焦点は駆動能力のみにあったため、次の機能は将来の作業として残されたことに注意してください。

フェイルセーフ
ヒューマン・マシン・インターフェース
リアルタイム処理
冗長化システム
状態監視システム
高レベルのアーキテクチャ設計
概要

Autoware のアーキテクチャは次の 6 つのスタックで構成されます。リンクされた各ページには、そのスタックに固有のより詳細な要件とユースケースのセットが含まれています。

センシング設計
地図デザイン
ローカリゼーション設計
知覚デザイン
企画デザイン
制御設計
車両インターフェース設計
ノード図
デフォルト構成の Autoware ノードを示す図は、ノード図ページにあります。各ノードの詳細なドキュメントは、Autoware Universe のドキュメントで入手できます。

Autoware 構成はスケーラブル/選択可能であり、環境と必要なユースケースによって異なることに注意してください。

参考文献
2020 年 3 月、AWF 技術運営委員会に対して行われたアーキテクチャのプレゼンテーション
# Architecture overview

This page describes the architecture of Autoware.

## Introduction

The current Autoware is defined to be a layered architecture that clarifies each module's role and simplifies the interface between them. By doing so:

- Autoware's internal processing becomes more transparent.
- Collaborative development is made easier because of the reduced interdependency between modules.
- Users can easily replace an existing module (e.g. localization) with their own software component by simply wrapping their software to fit in with Autoware's interface.

Note that the initial focus of this architecture design was solely on driving capability, and so the following features were left as future work:

- Fail safe
- Human Machine Interface
- Real-time processing
- Redundant system
- State monitoring system

## High-level architecture design

![Overview](image/autoware-architecture-overview.drawio.svg)

Autoware's architecture consists of the following six stacks. Each linked page contains a more detailed set of requirements and use cases specific to that stack:

- [Sensing design](sensing/index.md)
- [Map design](map/index.md)
- [Localization design](localization/index.md)
- [Perception design](perception/index.md)
- [Planning design](planning/index.md)
- [Control design](control/index.md)
- [Vehicle Interface design](vehicle/index.md)

## Node diagram

A diagram showing Autoware's nodes in the default configuration can be found on the [Node diagram](node-diagram/index.md) page. Detailed documents for each node are available in the [Autoware Universe docs](https://autowarefoundation.github.io/autoware.universe/main/).

Note that Autoware configurations are scalable / selectable and will vary depending on the environment and required use cases.

## References

- [The architecture presentation given to the AWF Technical Steering Committee, March 2020](https://discourse.ros.org/uploads/short-url/woUU7TGLPXFCTJLtht11rJ0SqCL.pdf)
