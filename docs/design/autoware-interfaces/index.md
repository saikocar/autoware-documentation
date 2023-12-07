Autoware インターフェイスの設計
抽象的な
Autoware では、3 つのカテゴリのインターフェイスが定義されています。1 つ目は、オペレーターや乗客がフリート管理システム (FMS) やヒューマン マシン インターフェイス (HMI) などの自動運転システムの外部から車両を操作するための Autoware AD API です。2 つ目は、コンポーネントが相互に通信するための Autoware コンポーネント インターフェイスです。最後のものは、コンポーネント内で使用されるローカル インターフェイスです。

コンセプト
アプリケーションは、複数のさまざまな車両を共通の方法で操作できます。

統合アクセス

アプリケーションは、バージョンの更新や実装の変更の影響を受けません。

変化を吸収する

開発者は、新しい機能やハードウェアを追加するためのインターフェイスを知るだけで済みます。

新機能

要件
目標:

AD API は、次のアプリケーションを作成する機能を提供します。
ルート上で車両を運転するか、指定された位置まで順番に運転します。
発進、停止などの車両の動作を操作します。
車両の状態を運転者、同乗者、周囲の人に表示またはアナウンスします。
ドアなどの車両機器を制御します。
車両を監視するか、手動で運転します。
AD API は安定した長期的な仕様を提供します。これにより、すべての車両への統一アクセスが可能になります。
AD API はバージョンや実装の違いを隠し、変更の影響を吸収します。
AD API にはデフォルトの実装があり、オプションを使用していくつかの単純な ODD に適用できます。
AD API 実装は、仕様を満たしている限り、サードパーティ コンポーネントを使用して拡張できます。
コンポーネント インターフェイスは、安定した中期的な仕様を提供します。これにより、コンポーネントの追加が容易になります。
コンポーネント インターフェイスは、コンポーネントのパブリック部分とプライベート部分を明確にし、保守性を向上させます。
コンポーネント インターフェイスはサードパーティの設計で拡張可能で、サブコンポーネントの再利用性が向上します。
目標以外:

AD API はセキュリティをカバーしません。他の信頼できる方法と組み合わせて使用​​してください。
コンポーネント インターフェイスは単なる仕様であり、実装は含まれません。
建築
Autoware のコンポーネントは、コンポーネント インターフェイスを介して接続されます。各コンポーネントはインターフェイスを使用して機能を提供し、他のコンポーネントにアクセスします。AD API 実装もコンポーネントです。AD APIに必要な機能要素はコンポーネントインターフェースとして定義されているため、他のコンポーネントはAD APIを直接考慮する必要がありません。シミュレーターなどの評価およびデバッグ用のツールは、AD API とコンポーネント インターフェイスの両方にアクセスします。

建築

コンポーネント インターフェイスには階層仕様があります。最上位のアーキテクチャはいくつかのコンポーネントで構成されます。各コンポーネントには、次のレベルのアーキテクチャのいくつかのオプションがあります。開発者は、コンポーネントを実装するときにそれらのいずれかを選択します。最も単純な次のレベルのアーキテクチャはモノリシックです。これはオールインワンのブラック ボックス実装であり、小グループの開発、プロトタイピング、および非常に複雑な機能に適しています。その他は、サブコンポーネントで構成される任意のアーキテクチャであり、大規模なグループ開発に利点があります。サブコンポーネントは、同じアーキテクチャを採用する他のコンポーネントと組み合わせることができます。サードパーティは、オープンソース開発用に独自のアーキテクチャとインターフェイスを定義して公開できます。十分に評価された場合には、標準化の提案を行うことが望ましい。

成分

特徴
通信方法
次の表に示すように、インターフェイスは 4 つの通信方式に分類され、その動作が定義されます。関数呼び出しはリクエストとレスポンスの通信であり、即時の結果が必要な処理に使用されます。その他はパブリッシュ/サブスクライブ通信です。通知は、何らかのイベント (通常はコールバック) によって変更されるデータを処理するために使用されます。ストリームは継続的に変化するデータを処理します。Reliable Stream はすべてのデータが損失なく到着することを期待し、Realtime Stream は最新のデータが低遅延で到着することを期待します。

通信方法	ROSの実装	オプションの実装
関数呼び出し	サービス	HTTP
通知	トピック (信頼できる、transient_local)	MQTT (QoS=2、保持)
信頼性の高いストリーム	トピック (信頼性、不安定性)	MQTT (QoS=2)
リアルタイムストリーム	トピック (ベストエフォート、揮発性)	MQTT (QoS=0)
Autoware は ROS を使用して開発され、主にそのパッケージと通信するため、これらのメソッドは ROS のサービスまたはトピックとして提供されます。一方、FMS や HMI は ROS なしで実装されることが多く、Autoware は ROS を使用しないアプリケーションと通信することも期待されています。これらのアプリケーションごとに Autoware 用のアダプターを持つのは無駄であり、より適切な通信手段が必要です。HTTP と MQTT は、広く使用されており、サービスやトピックの動作を置き換えることができるため、追加のオプションとして推奨されます。この場合、オブジェクトの配列内でフィールド名が繰り返される JSON などのテキスト形式は効率が悪く、シリアル化を考慮する必要があります。

命名規則
インターフェイスの名前は である必要があります/<component name>/api/<interface name>。 は<component name>コンポーネントの名前です。AD API コンポーネントの場合は、この部分を省略して から始めます/api。は<interface name>スラッシュで区切られた任意の文字列です。apiこのルールにより、名前空間を AD API およびコンポーネント インターフェイス以外の名前として使用してはならないという制限が生じることに注意してください。

以下は、AD API とコンポーネント インターフェイスの正しいインターフェイス名の例です。

/api/autoware/状態
/api/autoware/engage
/planning/api/route/set
/車両/API/ステータス
次に、AD API およびコンポーネント インターフェイスの誤ったインターフェイス名の例を示します。

/ad_api/autoware/state
/autoware/エンゲージ
/計画/ルート/セット/API
/vehicle/my_api/ステータス
ロギング
車両の挙動を分析するためにインターフェイスをログに記録することをお勧めします。ロギングが必要な場合は、トピックには rosbag を使用でき、サービスには rclcpp または rclpy のロガーを使用します。通常は、サービスの呼び出し時にログを記録するサービスおよびクライアント クラスのラッパーを作成します。

制限
APIごとに以下のような制限事項を考慮し、必要に応じて記述してください。

サービス:

反応時間
前提条件
事後条件
実行順序
同時実行
トピック：

推奨遅延範囲
最大遅延
推奨周波数範囲
最小周波数
デフォルトの頻度
データ構造
データ型の定義
ある API の変更が別の API に影響を与えることを避けるために、明らかに同じでない限り、AD API の型を共有しないでください。また、同じ理由で、コンポーネント インターフェイスを含む実装依存の型を AD API で使用しないでください。実装時にAD APIの型を使用するか、同じ型を作成してデータをコピーして型を変換します。

定数と列挙型
ROS は列挙をサポートしていないため、代わりに定数を使用してください。変数が割り当てられていないことを検出するために、ゼロや空の文字列などの型のデフォルト値を使用しないでください。あるいは、未定義であることを示す専用の名前を割り当てます。1 つの型に複数の列挙型がある場合は、定数と変数の対応についてコメントしてください。バージョンが更新されると割り当てが変更される可能性があるため、列挙値を直接使用しないでください。

タイムスタンプ
タイムスタンプが何を示しているかを明確にします。たとえば、送信時間、測定時間、更新時間などです。必要に応じて、複数のタイムスタンプを持つことを検討してください。ROS変換を使用する場合に使用しますstd_msgs/msg/Header。また、ヘッダーがすべてのデータに共通であるか、データごとに独立しているか、または追加のタイムスタンプが必要であるかどうかも考慮してください。

リクエストヘッダー
現時点では、必要なヘッダーはありません。

対応状況
通信方式が Function Call のインタフェースは、エラーの形式を統一するために共通の応答ステータスを使用します。これらのインターフェイスには、応答に status という名前の ResponseStatus 変数を含める必要があります。詳細については、autoware_adapi_v1_msgs/msg/ResponseStatusを参照してください。

懸念、仮定、制限
アプリケーションは、AD API によって提供されるバージョン情報を使用して互換性を確認します。不明なバージョンも、メジャー バージョンが一致する限り利用可能として扱われます (メジャー バージョン 0 を除く)。AD API とコンポーネント インターフェイス間の互換性は、バージョン管理システムによって維持されると想定されます。
AD API の意図しない動作が検出された場合、アプリケーションは適切なアクションを実行する必要があります。Autoware は可能な限り長く動作し続けるよう努めますが、安全性は保証されません。安全性はアプリケーションを含むシステム全体で考慮する必要があります。
# Autoware interface design

## Abstract

Autoware defines three categories of interfaces. The first one is Autoware AD API for operating the vehicle from outside the autonomous driving system such as the Fleet Management System (FMS) and Human Machine Interface (HMI) for operators or passengers. The second one is Autoware component interface for components to communicate with each other. The last one is the local interface used inside the component.

## Concept

- Applications can operate multiple and various vehicles in a common way.

  ![unified-access](./general/unified-access.drawio.svg)

- Applications are not affected by version updates and implementation changes.

  ![absorb-changes](./general/absorb-changes.drawio.svg)

- Developers only need to know the interface to add new features and hardware.

  ![new-feature](./general/new-feature.drawio.svg)

## Requirements

Goals:

- AD API provides functionality to create the following applications:
  - Drive the vehicle on the route or drive to the requested positions in order.
  - Operate vehicle behavior such as starting and stopping.
  - Display or announce the vehicle status to operators, passengers, and people around.
  - Control vehicle devices such as doors.
  - Monitor the vehicle or drive it manually.
- AD API provides stable and long-term specifications. This enables unified access to all vehicles.
- AD API hides differences in version and implementation and absorbs the impact of changes.
- AD API has a default implementation and can be applied to some simple ODDs with options.
- The AD API implementation is extensible with the third-party components as long as it meets the specifications.
- The component interface provides stable and medium-term specifications. This makes it easier to add components.
- The component interface clarifies the public and private parts of a component and improves maintainability.
- The component interface is extensible with the third-party design to improve the sub-components' reusability.

Non-goals:

- AD API does not cover security. Use it with other reliable methods.
- The component interface is just a specification, it does not include an implementation.

## Architecture

The components of Autoware are connected via the component interface.
Each component uses the interface to provide functionality and to access other components.
AD API implementation is also a component.
Since the functional elements required for AD API are defined as the component interface, other components do not need to consider AD API directly.
Tools for evaluation and debugging, such as simulators, access both AD API and the component interface.

![architecture](./general/architecture.drawio.svg)

The component interface has a hierarchical specification.
The top-level architecture consists of some components. Each component has some options of the next-level architecture.
Developers select one of them when implementing the component. The simplest next-level architecture is monolithic.
This is an all-in-one and black box implementation, and is suitable for small group development, prototyping, and very complex functions.
Others are arbitrary architecture consists of sub-components and have advantages for large group development.
A sub-component can be combined with others that adopt the same architecture.
Third parties can define and publish their own architecture and interface for open source development.
It is desirable to propose them for standardization if they are sufficiently evaluated.

![component](./general/hierarchy.drawio.svg)

## Features

### Communication methods

As shown in the table below, interfaces are classified into four communication methods to define their behavior.
Function Call is a request-response communication and is used for processing that requires immediate results. The others are publish-subscribe communication.
Notification is used to process data that changes with some event, typically a callback. Streams handle continuously changing data.
Reliable Stream expects all data to arrive without loss, Realtime Stream expects the latest data to arrive with low delay.

| Communication Method | ROS Implementation                | Optional Implementation |
| -------------------- | --------------------------------- | ----------------------- |
| Function Call        | Service                           | HTTP                    |
| Notification         | Topic (reliable, transient_local) | MQTT (QoS=2, retain)    |
| Reliable Stream      | Topic (reliable, volatile)        | MQTT (QoS=2)            |
| Realtime Stream      | Topic (best_effort, volatile)     | MQTT (QoS=0)            |

These methods are provided as services or topics of ROS since Autoware is developed using ROS and mainly communicates with its packages.
On the other hand, FMS and HMI are often implemented without ROS, Autoware is also expected to communicate with applications that do not use ROS.
It is wasteful for each of these applications to have an adapter for Autoware, and a more suitable means of communication is required.
HTTP and MQTT are suggested as additional options because these protocols are widely used and can substitute the behavior of services and topics.
In that case, text formats such as JSON where field names are repeated in an array of objects, are inefficient and it is necessary to consider the serialization.

### Naming convention

The name of the interface must be `/<component name>/api/<interface name>`,
where `<component name>` is the name of the component. For an AD API component, omit this part and start with `/api`.
The `<interface name>` is an arbitrary string separated by slashes.
Note that this rule causes a restriction that the namespace `api` must not be used as a name other than AD API and the component interface.

The following are examples of correct interface names for AD API and the component interface:

- /api/autoware/state
- /api/autoware/engage
- /planning/api/route/set
- /vehicle/api/status

The following are examples of incorrect interface names for AD API and the component interface:

- /ad_api/autoware/state
- /autoware/engage
- /planning/route/set/api
- /vehicle/my_api/status

### Logging

It is recommended to log the interface for analysis of vehicle behavior.
If logging is needed, rosbag is available for topics, and use logger in rclcpp or rclpy for services.
Typically, create a wrapper for service and client classes that logs when a service is called.

### Restrictions

For each API, consider the restrictions such as following and describe them if necessary.

Services:

- response time
- pre-condition
- post-condition
- execution order
- concurrent execution

Topics:

- recommended delay range
- maximum delay
- recommended frequency range
- minimum frequency
- default frequency

## Data structure

### Data type definition

Do not share the types in AD API unless they are obviously the same to avoid changes in one API affecting another.
Also, implementation-dependent types, including the component interface, should not be used in AD API for the same reason.
Use the type in AD API in implementation, or create the same type and copy the data to convert the type.

### Constants and enumeration

Since ROS don't support enumeration, use constants instead.
The default value of type such as zero and empty string should not be used to detect that a variable is unassigned.
Alternatively, assign it a dedicated name to indicate that it is undefined.
If one type has multiple enumerations, comment on the correspondence between constants and variables.
Do not use enumeration values directly, as assignments are subject to change when the version is updated.

### Time stamp

Clarify what the timestamp indicates. for example, send time, measurement time, update time, etc. Consider having multiple timestamps if necessary.
Use `std_msgs/msg/Header` when using ROS transform.
Also consider whether the header is common to all data, independent for each data, or additional timestamp is required.

### Request header

Currently, there is no required header.

### Response status

The interfaces whose communication method is Function Call use a common response status to unify the error format.
These interfaces should include a variable of ResponseStatus with the name status in the response.
See [autoware_adapi_v1_msgs/msg/ResponseStatus](https://github.com/autowarefoundation/autoware_adapi_msgs/tree/main/autoware_adapi_v1_msgs#responsestatus) for details.

## Concerns, assumptions and limitations

- The applications use the version information provided by AD API to check compatibility.
  Unknown versions are also treated as available as long as the major versions match (excluding major version 0).
  Compatibility between AD API and the component interface is assumed to be maintained by the version management system.
- If an unintended behavior of AD API is detected, the application should take appropriate action.
  Autoware tries to keep working as long as possible, but it is not guaranteed to be safe.
  Safety should be considered for the entire system, including the applications.
