オートウェアの概念
Autoware は、自動運転システム用の世界初のオープンソース ソフトウェアです。Autoware は両方の価値を提供します。自動運転システムの技術開発者は、Autoware に基づいて新しいコンポーネントを作成できます。一方、自動運転システムのサービスオペレーターは、Autoware を使用して適切なテクノロジーコンポーネントを選択できます。これは、ソフトウェア スタックをコア サブシステムとユニバース サブシステム (モジュール) にモジュール化するマイクロオートノミー アーキテクチャによって実現されます。

マイクロオートノミーアーキテクチャ
Autoware はパイプライン アーキテクチャを使用して自動運転システムの開発を可能にします。Autoware で使用されるパイプライン アーキテクチャは、3 層アーキテクチャと同様のコンポーネントで構成されます。そしてそれらは並行して実行されます。コアとユニバースという 2 つの主要なモジュールがあります。これらのモジュールのコンポーネントは、拡張可能で再利用できるように設計されています。私たちはそれをマイクロオートノミー アーキテクチャと呼んでいます。

コアとユニバース.svg

コアモジュール
コア モジュールには、自動運転システムに必要なセンシング、コンピューティング、作動の基本的な機能と能力を満たす基本的なランタイムとテクノロジ コンポーネントが含まれています。AWF は、アーキテクトおよびワーキング グループを通じて主要なメンバーとともにコア モジュールを開発および保守します。コアには誰でも貢献できますが、PR (プル リクエスト) の受け入れ基準はユニバースと比較してより厳格です。

ユニバースモジュール
Universe モジュールは、センシング、コンピューティング、および作動の機能と能力を強化するためにテクノロジー開発者が提供できる Core モジュールの拡張機能です。AWF は、拡張元となる基本 Universe モジュールを提供します。マイクロオートノミー アーキテクチャの重要な特徴は、Universe モジュールをあらゆる組織や個人が貢献できることです。つまり、ユニバースを作成して、Autoware コミュニティやエコシステムで利用できるようにすることもできます。AWF は、開発プロセスを通じて Universe モジュールの品質管理を担当します。その結果、Universe モジュールには複数のタイプがあり、AWF によって検証および検証されるものと、そうでないものがあります。最終アプリケーションを構築するためにどの Universe モジュールを選択して統合するかは、Autoware のユーザー次第です。

インターフェース設計
インターフェイス設計はマイクロオートノミー アーキテクチャの最も重要な部分であり、内部インターフェイスと外部インターフェイスに分類されます。コンポーネント インターフェイスは、Universe モジュール内のコンポーネントが Autoware 内部でコア モジュールを含む他のモジュール内のコンポーネントと通信できるように設計されています。一方、AD (Autonomous Driving) API は、Autoware のアプリケーションが Autoware のコア モジュールとユニバース モジュールのテクノロジ コンポーネントに外部からアクセスできるように設計されています。堅牢なインターフェイスを設計することにより、マイクロオートノミー アーキテクチャは AWF のパートナーとともに実現され、同時にパートナーにとっても実現可能になります。

課題
マイクロオートノミー アーキテクチャの大きな課題は、システム内でアクティブ化されているすべてのテクノロジー コンポーネントが予測どおりにタイミング制約 (指定された期限) を満たすことを保証するリアルタイム機能を実現することです。一般に、コンポーネントの最悪の場合の実行時間 (WCET) を厳密に推定することは、不可能ではないにしても困難です。

さらに、DAG によって接続されているコンポーネントのエンドツーエンドの遅延を厳密に推定することも、不可能ではないにしても困難です。したがって、マイクロオートノミー アーキテクチャに基づく自動運転システムは、絶対に失敗しないように設計する必要はありますが、フェールセーフになるように設計する必要があります。オーバーランを考慮する限り、タイミング制約に違反する可能性がある (指定された期限を守れない可能性がある) ことを受け入れます。オーバーラン ハンドラーには、(i) プラットフォーム定義と (ii) ユーザー定義の 2 つがあります。プラットフォーム定義のハンドラーはデフォルトでプラットフォームの一部として実装されますが、ユーザー定義のハンドラーはそれを上書きしたり、新しいハンドラーをシステムに追加したりできます。これは、タイムリーな「フェイルセーフ」と呼ばれるものです。

要件とロードマップ
目標:

すべてオープンソース
ユースケース主導型
オーバーラン処理を備えたリアルタイム (予測可能な) フレームワーク
コードの品質
# Autoware concepts

Autoware is the world’s first open-source software for autonomous driving systems. Autoware provides value for both The technology developers of autonomous driving systems can create new components based on Autoware. The service operators of autonomous driving systems, on the other hand, can select appropriate technology components with Autoware. This is enabled by the microautonomy architecture that modularizes its software stack into the core and universe subsystems (modules).

## Microautonomy architecture

Autoware uses a [pipeline architecture](http://www.cs.sjsu.edu/~pearce/modules/patterns/distArch/pipeline.htm) to enable the development of autonomous driving systems. The pipeline architecture used in Autoware consists of components similar to [three-layer-architecture](http://www.flownet.com/gat/papers/tla.pdf). And they run in parallel. There are 2 main modules: the Core and the Universe. The components in these modules are designed to be extensible and reusable. And we call it microautonomy architecture.

![core-and-universe.svg](core-and-universe.svg)

### The Core module

The Core module contains basic runtimes and technology components that satisfy the basic functionality and capability of sensing, computing, and actuation required for autonomous driving systems. AWF develops and maintains the Core module with their architects and leading members through their working groups. Anyone can contribute to the Core but the PR(Pull Request) acceptance criteria is more strict compared to the Universe.

### The Universe module

The Universe modules are extensions to the Core module that can be provided by the technology developers to enhance the functionality and capability of sensing, computing, and actuation. AWF provides the base Universe module to extend from. A key feature of the microautonomy architecture is that the Universe modules can be contributed to by any organization and individual. That is, you can even create your Universe and make it available for the Autoware community and ecosystem. AWF is responsible for quality control of the Universe modules through their development process. As a result, there are multiple types of the Universe modules - some are verified and validated by AWF and others are not. It is up to the users of Autoware which Universe modules are selected and integrated to build their end applications.

## Interface design

The interface design is the most essential piece of the microautonomy architecture, which is classified into internal and external interfaces. The component interface is designed for the components in a Universe module to communicate with those in other modules, including the Core module, within Autoware internally. The AD(Autonomous Driving) API, on the other hand, is designed for the applications of Autoware to access the technology components in the Core and Universe modules of Autoware externally. Designing solid interfaces, the microautonomy architecture is made possible with AWF's partners, and at the same time is made feasible for the partners.

## Challenges

A grand challenge of the microautonomy architecture is to achieve real-time capability, which guarantees all the technology components activated in the system to predictably meet timing constraints (given deadlines). In general, it is difficult, if not impossible, to tightly estimate the worst-case execution times (WCETs) of components.

In addition, it is also difficult, if not impossible, to tightly estimate the end-to-end latency of components connected by a DAG. Autonomous driving systems based on the microautonomy architecture, therefore, must be designed to be fail-safe but not never-fail. We accept that the timing constraints may be violated (the given deadlines may be missed) as far as the overrun is taken into account. The overrun handlers are two-fold: (i) platform-defined and (ii) user-defined. The platform-defined handler is implemented as part of the platform by default, while the user-defined handler can overwrite it or add a new handler to the system. This is what we call “fail-safe” on a timely basis.

## Requirements and roadmap

Goals:

- All open-source
- Use case driven
- Real-time (predictable) framework with overrun handling
- Code quality
