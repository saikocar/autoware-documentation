Autoware Core/Universe は Autoware.AI および Autoware.Auto とどのように異なりますか?
Autoware は、世界初の自動運転車用の「オールインワン」オープンソース ソフトウェアです。2015 年に初めてリリースされて以来、異なる基本コンセプトで複数のリリースが作成され、それぞれがソフトウェアの改善を目的としていました。

Autoware.AI
Autoware.AI は、ROS 1 に基づいてリリースされた Autoware の最初のディストリビューションです。リポジトリには、センシング、作動、位置特定、マッピング、認識、計画など、自動運転技術のさまざまな側面をカバーするさまざまなパッケージが含まれています。

Autoware.AI は多くの開発者と貢献を引き付けることに成功しましたが、次のような理由により、Autoware.AI の機能を改善するのは困難でした。

具体的なアーキテクチャ設計が欠如しているため、モジュール間の緊密な結合やモジュールの責任の不明確さなど、多くの技術的負債が蓄積されます。
パッケージごとにコーディング標準が異なり、テストカバレッジが非常に低い。
さらに、Autoware 対応の自動運転車が動作できる条件や、サポートされるユースケースや状況 (静止車両を追い越す機能など) についての明確な定義はありませんでした。

Autoware.AI 開発から学んだ教訓から、Autoware.Auto では異なる開発プロセスが採用され、Autoware の ROS 2 バージョンが開発されました。

!!! 警告

Autoware.AI is currently in maintenance mode and will reach end-of-life at the end of 2022.
Autoware.Auto
Autoware.Auto は、ROS 2 に基づいてリリースされた Autoware の 2 番目のディストリビューションです。ROS 2 への移行の一環として、Autoware.AI を ROS 1 から ROS 2 に単純に移植することは避けることが決定されました。代わりに、コードベースは ROS 2 から書き直されました。ターゲットのユースケースと ODD (例: 自動バレーパーキング [AVP]、貨物配送など) の定義、適切なアーキテクチャの設計、設計ドキュメントとテスト コードの作成など、適切なエンジニアリング プラクティスを一から実行します。

Autoware.Auto の開発は当初は問題なく機能しているように見えましたが、AVP プロジェクトと Cargo Delivery ODD プロジェクトが完了した後、次の問題が発生し始めました。

新人エンジニアへの障壁は高すぎました。
新しい機能を Autoware.Auto にマージするには多くの作業が必要であったため、研究者や学生が開発に貢献するのは困難でした。
その結果、ほとんどの Autoware.Auto 開発者は Autoware Foundation の企業の出身であったため、研究論文から最先端の機能を追加できる人はほとんどいませんでした。
大規模なアーキテクチャ変更は非常に困難でした。
実験的なアーキテクチャを試すには、メイン ブランチの安定性を維持しながら、すべての変更が継続的インテグレーションの要件を満たしていることを確認するために、非常に大きなオーバーヘッドが発生しました。
Autoware コア/ユニバース
Autoware.Auto 開発の問題に対処するために、Autoware Foundation は Autoware Core/Universe と呼ばれる新しいアーキテクチャを作成することを決定しました。

Autoware Core は、Autoware.Auto の元のポリシーを引き継ぎ、安定した十分にテストされたコードベースになります。Autoware Core に加えて、Autoware Universe と呼ばれる新しい概念もあり、Autoware Core の拡張機能として機能し、次の利点があります。

ユーザーは、新しいローカリゼーション アルゴリズムや知覚アルゴリズムなどのより高度な機能を使用するために、コア コンポーネントを同等のユニバースと簡単に置き換えることができます。
Universe のコード品質要件は、新しい開発者、学生、研究者が貢献しやすくするためにより緩和されていますが、それでも Autoware.AI の要件よりは厳格です。
Universe に追加された、より広範な Autoware コミュニティに役立つ高度な機能はすべてレビューされ、メインの Autoware Core コードベースに組み込む可能性が検討されます。
このようにして、安定した安全な自動運転システムを実現するという主な要件を達成できると同時に、サードパーティの貢献者によって作成された最先端の機能へのアクセスが可能になります。Autoware Core/Universe の設計の詳細については、Autoware の概念ドキュメント ページを参照してください。
# How is Autoware Core/Universe different from Autoware.AI and Autoware.Auto?

Autoware is the world's first "all-in-one" open-source software for self-driving vehicles.
Since it was first released in 2015, there have been multiple releases made with differing underlying concepts, each one aimed at improving the software.

## Autoware.AI

[Autoware.AI](https://github.com/Autoware-AI/autoware.ai) is the first distribution of Autoware that was released based on ROS 1. The repository contains a variety of packages covering different aspects of autonomous driving technologies - sensing, actuation, localization, mapping, perception and planning.

While it was successful in attracting many developers and contributions, it was difficult to improve Autoware.AI's capabilities for a number of reasons:

- A lack of concrete architecture design leading to a lot of built-up technical debt, such as tight coupling between modules and unclear module responsibility.
- Differing coding standards for each package, with very low test coverage.

Furthermore, there was no clear definition of the conditions under which an Autoware-enabled autonomous vehicle could operate, nor of the use cases or situations supported (eg: the ability to overtake a stationary vehicle).

From the lessons learned from Autoware.AI development, a different development process was taken for Autoware.Auto to develop a ROS 2 version of Autoware.

!!! warning

    Autoware.AI is currently in maintenance mode and will reach end-of-life at the end of 2022.

## Autoware.Auto

[Autoware.Auto](https://gitlab.com/autowarefoundation/autoware.auto/AutowareAuto) is the second distribution of Autoware that was released based on ROS 2. As part of the transition to ROS 2, it was decided to avoid simply porting Autoware.AI from ROS 1 to ROS 2. Instead, the codebase was rewritten from scratch with proper engineering practices, including defining target use cases and ODDs (eg: Autonomous Valet Parking [AVP], Cargo Delivery, etc.), designing a proper architecture, writing design documents and test code.

Autoware.Auto development seemed to work fine initially, but after completing the AVP and and Cargo Delivery ODD projects, we started to see the following issues:

- The barrier to new engineers was too high.
  - A lot of work was required to merge new features into Autoware.Auto, and so it was difficult for researchers and students to contribute to development.
  - As a consequence, most Autoware.Auto developers were from companies in the Autoware Foundation and so there were very few people who were able to add state-of-the-art features from research papers.
- Making large-scale architecture changes was too difficult.
  - To try out experimental architecture, there was a very large overhead involved in keeping the main branch stable whilst also making sure that every change satisfied the continuous integration requirements.

## Autoware Core/Universe

In order to address the issues with Autoware.Auto development, the Autoware Foundation decided to create a new architecture called Autoware Core/Universe.

Autoware Core carries over the original policy of Autoware.Auto to be a stable and well-tested codebase. Alongside Autoware Core is a new concept called Autoware Universe, which acts as an extension of Autoware Core with the following benefits:

- Users can easily replace a Core component with a Universe equivalent in order to use more advanced features, such as a new Localization or Perception algorithm.
- Code quality requirements for Universe are more relaxed to make it easier for new developers, students and researchers to contribute, but will still be stricter than the requirements for Autoware.AI.
- Any advanced features added to Universe that are useful to the wider Autoware community will be reviewed and considered for potential inclusion in the main Autoware Core codebase.

This way, the primary requirement of having a stable and safe autonomous driving system can be achieved, whilst simultaneously enabling access to state-of-the-art features created by third-party contributors. For more details about the design of Autoware Core/Universe, refer to the [Autoware concepts documentation page](../autoware-concepts/).
