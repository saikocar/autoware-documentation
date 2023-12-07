Autoware のデザイン
建築
コアとユニバース。

Autoware は、オープンソース ソフトウェアによってランタイムとテクノロジ コンポーネントを提供します。ランタイムはロボット オペレーティング システム (ROS) に基づいています。テクノロジー コンポーネントは貢献者によって提供されます。これには以下が含まれますが、これらに限定されません。

センシング
カメラコンポーネント
LiDAR コンポーネント
レーダーコンポーネント
GNSSコンポーネント
コンピューティング
ローカリゼーションコンポーネント
知覚コンポーネント
計画コンポーネント
制御コンポーネント
ロギングコンポーネント
システム監視コンポーネント
作動
DBWコンポーネント
ツール
シミュレータコンポーネント
マッピングコンポーネント
リモートコンポーネント
ML コンポーネント
注釈コンポーネント
校正コンポーネント
懸念、仮定、および制限
マイクロオートノミー アーキテクチャの欠点は、機能モジュール性に起因するデータ パスのオーバーヘッドにより、エンド アプリケーションの計算パフォーマンスが犠牲になることです。言い換えれば、マイクロオートノミー アーキテクチャのトレードオフ特性は、計算パフォーマンスと機能モジュール性の間に存在します。このトレードオフの問題は、リアルタイム機能を導入することで技術的に解決できます。これは、自動運転システムが実際には高速になるように設計されていないためです。つまり、低遅延コンピューティングはあれば便利ですが、必須ではありません。自動運転システムに必須の機能は、コンピューティングの遅延が予測可能であること、つまりシステムがリアルタイムであることです。全体として、自動運転システムの所定のタイミング制約 (計算のデッドラインと呼ばれることが多い) を満たすのに十分な予測可能な範囲で、計算パフォーマンスを妥協することができます。

デザイン
!!! 警告

Under Construction
オートウェアの概念
Autowareの概念ページでは、 Autoware の設計哲学について説明します。読者 (サービス プロバイダーおよびすべての Autoware ユーザー) は、マイクロオートノミーやコア/ユニバース アーキテクチャなど、Autoware 開発の基礎となる基本概念を学びます。

オートウェアのアーキテクチャ
Autowareアーキテクチャのページでは、 Autoware を構成する各モジュールの概要について説明します。読者 (すべての Autoware ユーザー) は、Autoware を構成する各モジュールがどのように機能するかの概要を理解できます。

オートウェアインターフェイス
Autowareインターフェイスのページでは、 Autoware を構成する各モジュールのインターフェイスについて詳しく説明します。読者 (中級開発者) は、Autoware に新しい機能を追加する方法と、独自のモジュールを Autoware に統合する方法を学びます。

構成管理
結論
# Autoware's Design

## Architecture

Core and Universe.

Autoware provides the runtimes and technology components by open-source software. The runtimes are based on the Robot Operating System (ROS). The technology components are provided by contributors, which include, but are not limited to:

- Sensing
  - Camera Component
  - LiDAR Component
  - RADAR Component
  - GNSS Component
- Computing
  - Localization Component
  - Perception Component
  - Planning Component
  - Control Component
  - Logging Component
  - System Monitoring Component
- Actuation
  - DBW Component
- Tools
  - Simulator Component
  - Mapping Component
  - Remote Component
  - ML Component
  - Annotation Component
  - Calibration Component

## Concern, Assumption, and Limitation

The downside of the microautonomy architecture is that the computational performance of end applications is sacrificed due to its data path overhead attributed to functional modularity. In other words, the trade-off characteristic of the microautonomy architecture exists between computational performance and functional modularity. This trade-off problem can be solved technically by introducing real-time capability. This is because autonomous driving systems are not really designed to be real-fast, that is, low-latency computing is nice-to-have but not must-have. The must-have feature for autonomous driving systems is that the latency of computing is predictable, that is, the systems are real-time. As a whole, we can compromise computational performance to an extent that is predictable enough to meet the given timing constraints of autonomous driving systems, often referred to as deadlines of computation.

## Design

!!! warning

    Under Construction

### Autoware concepts

The [Autoware concepts page](autoware-concepts/index.md) describes the design philosophy of Autoware. Readers (service providers and all Autoware users) will learn the basic concepts underlying Autoware development, such as microautonomy and the Core/Universe architecture.

### Autoware architecture

The [Autoware architecture page](autoware-architecture/index.md) describes an overview of each module that makes up Autoware. Readers (all Autoware users) will gain a high-level picture of how each module that composes Autoware works.

### Autoware interfaces

The [Autoware interfaces page](autoware-interfaces/index.md) describes in detail the interface of each module that makes up Autoware. Readers (intermediate developers) will learn how to add new functionality to Autoware and how to integrate their own modules with Autoware.

### Configuration management

## Conclusion
