知覚コンポーネントの設計
この文書の目的
このドキュメントでは、Perception コンポーネントの開発における高レベルの設計戦略、目標、および関連する理論的根拠について概説します。この文書を通じて、すべての OSS 開発者が Perception コンポーネントの設計哲学、目標、制約を理解し、開発にシームレスに参加できることが期待されます。

概要
知覚コンポーネントは、センシング、ローカリゼーション、およびマップ コンポーネントから入力を受け取り、意味論的な情報 (例: オブジェクト認識、障害物セグメント化、交通信号認識、占有グリッド マップ) を追加し、その後、計画コンポーネントに渡されます。このコンポーネントの設計は、マイクロオートノミーの概念として定義される Autoware の包括的な哲学に従っています。

目標と非目標
Perception Componentの役割は、センシングで得られたデータをもとに周囲の環境を認識し、自動運転に必要な情報（動的物体の存在、静止障害物、死角、信号情報など）を取得することです。

私たちの全体的な設計では、マイクロオートノミー アーキテクチャの概念を重視しています。この用語は、機能を適切にモジュール化し、それらのモジュール間のインターフェイスを明確に定義し、その結果としてシステムの高い拡張性を実現することに重点を置いた設計アプローチを指します。この状況を考慮すると、Perception コンポーネントの目標は、考えられるすべての複雑なユースケースを解決することではなく (ただし、基本的なユースケースをサポートすることを目指しています)、むしろユーザーのニーズに合わせてカスタマイズでき、開発を促進できるプラットフォームを提供することに設定されています。追加機能の。

設計コンセプトを明確にするために、目標と非目標として次の点を列挙します。

目標:

単純な ODD を定義できるように、基本的な関数を提供します。
すべての自動運転車に知覚機能を提供できる設計を実現する。
サードパーティのコンポーネントを使用して拡張できるようにするため。
Autoware ユーザーが完全な機能を開発できるプラットフォームを提供する。
Autoware ユーザーが人間のドライバーを常に上回る自動運転システムを開発できるプラットフォームを提供すること。
Autoware ユーザーが「100% の精度」または「エラーのない認識」を実現する自動運転システムを開発できるプラットフォームを提供する。
目標以外:

特定/限定された ODD に特化した知覚コンポーネント アーキテクチャを開発する。
完全な機能と能力を実現するため。
人間のドライバーの認識能力を上回る。
「100％の精度」あるいは「誤りのない認識」を実現するために。
高レベルのアーキテクチャ
この図は、Perception コンポーネントの高レベルのアーキテクチャを示しています。

全体的な認識のアーキテクチャ

Perception コンポーネントは次のサブコンポーネントで構成されます。

物体認識: 現在のフレーム内で自車両の周囲にある動的物体を認識し、それらの将来の軌道を予測します。
障害物セグメンテーション: 自車両が回避すべき障害物 (動的オブジェクトだけでなく、静止障害物などの回避すべき静的障害物も含む) に由来する点群を識別します。
占有グリッド マップ: 死角 (情報が入手できず、動的オブジェクトが飛び出す可能性のある領域) を検出します。
信号機認識: 信号機の色と矢印信号の方向を認識します。
コンポーネントインターフェース
以下に、Perception Component と他のコンポーネント間の入出力の概念を説明します。現在の実装については、「Perception Component Interface」ページを参照してください。

知覚コンポーネントへの入力
From Sensing : この入力は、環境に関するリアルタイムの情報を提供する必要があります。
カメラ画像：カメラから取得した画像データ。
点群: LiDAR から取得された点群データ。
レーダーオブジェクト: レーダーから取得されたオブジェクトデータ。
From Localization : この入力は、自車両に関するリアルタイム情報を提供する必要があります。
車両運動情報：自車両の位置を含む。
From Map : この入力は、環境に関する静的情報に関するリアルタイム情報を提供する必要があります。
ベクター マップ: レーン アリア情報を含む、環境に関するすべての静的情報が含まれます。
点群マップ: 静的な点群マップが含まれますが、動的オブジェクトに関する情報は含まれません。
API から:
V2X 情報: V2X モジュールからの情報。例えば信号機からの情報。
知覚コンポーネントからの出力
企画へ
動的オブジェクト: 歩行者や他の車両など、事前に知ることができないオブジェクトに関するリアルタイムの情報を提供します。
障害物のセグメンテーション: 検出されたオブジェクトよりも原始的な、障害物の位置に関するリアルタイム情報を提供します。
占有グリッド マップ: 遮蔽領域情報の存在に関するリアルタイム情報を提供します。
信号機の認識結果: 各信号機の現在の状態をリアルタイムで提供します。
新しいモジュールを追加する方法 (WIP)
目標セッションで述べたように、この認識モジュールはサードパーティのコンポーネントによって拡張できるように設計されています。新しいモジュールを追加してその機能を拡張する方法の具体的な手順については、提供されているドキュメントまたはガイドライン (WIP) を参照してください。

サポートされている機能
特徴	説明	要件
LiDAR DNN ベースの 3D 検出器	このモジュールは点群を入力として受け取り、車両、トラック、バス、歩行者、自転車などのオブジェクトを検出します。	- 点群
カメラ DNN ベースの 2D 検出器	このモジュールは、カメラ画像を入力として受け取り、2 次元画像空間内の車両、トラック、バス、歩行者、自転車などのオブジェクトを検出します。画像座標内のオブジェクトを検出しますが、3D 座標情報の提供は必須ではありません。	- カメラ画像
LiDAR クラスタリング	このモジュールは、点群のクラスタリングと形状推定を実行して、ラベルなしのオブジェクト検出を実現します。	- 点群
準ルールベースの検出器	このモジュールは、画像と点群の両方からの情報を使用してオブジェクトを検出し、LiDAR クラスタリングとカメラ DNN ベースの 2D 検出器の 2 つのコンポーネントで構成されます。	- カメラ DNN ベースの 2D 検出器と LiDAR クラスタリングからの出力
オブジェクトのマージ	このモジュールは、さまざまな検出器からの結果を統合します。	- 検出されたオブジェクト
補間器	このモジュールは、追跡結果を使用して検出結果を長期間維持することで、物体検出結果を安定させます。	- 検出されたオブジェクト
- 追跡されたオブジェクト
追跡	検出結果にIDと推定速度を付与するモジュールです。	- 検出されたオブジェクト
予測	このモジュールは、地図の形状と周囲の環境に応じて、動的オブジェクトの将来の経路 (およびその確率) を予測します。	- 追跡対象物
- ベクトルマップ
障害物のセグメンテーション	このモジュールは、自車両が回避すべき障害物に由来する点群を識別します。	- 点群
- 点群マップ
占有グリッドマップ	このモジュールは死角 (情報が入手できず、動的物体が飛び出す可能性のある領域) を検出します。	- 点群
- 点群マップ
信号機の認識	信号機の位置と状態を検出するモジュールです。	- カメラ画像
- ベクトルマップ
リファレンス実装
Autoware が起動されると、デフォルトのパラメータがロードされ、リファレンス実装が開始されます。詳細については、「リファレンス実装」を参照してください。
# Perception Component Design

## Purpose of this document

This document outlines the high-level design strategies, goals and related rationales in the development of the Perception Component. Through this document, it is expected that all OSS developers will comprehend the design philosophy, goals and constraints under which the Perception Component is designed, and participate seamlessly in the development.

## Overview

The Perception Component receives inputs from Sensing, Localization, and Map components, and adds semantic information (e.g., Object Recognition, Obstacle Segmentation, Traffic Light Recognition, Occupancy Grid Map), which is then passed on to Planning Component. This component design follows the overarching philosophy of Autoware, defined as the [microautonomy concept](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-concepts/).

## Goals and non-goals

The role of the Perception Component is to recognize the surrounding environment based on the data obtained through Sensing and acquire sufficient information (such as the presence of dynamic objects, stationary obstacles, blind spots, and traffic signal information) to enable autonomous driving.

In our overall design, we emphasize the concept of [microautonomy architecture](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-concepts). This term refers to a design approach that focuses on the proper modularization of functions, clear definition of interfaces between these modules, and as a result, high expandability of the system. Given this context, the goal of the Perception Component is set not to solve every conceivable complex use case (although we do aim to support basic ones), but rather to provide a platform that can be customized to the user's needs and can facilitate the development of additional features.

To clarify the design concepts, the following points are listed as goals and non-goals.

**Goals:**

- To provide the basic functions so that a simple ODD can be defined.
- To achieve a design that can provide perception functionality to every autonomous vehicle.
- To be extensible with the third-party components.
- To provide a platform that enables Autoware users to develop the complete functionality and capability.
- To provide a platform that enables Autoware users to develop the autonomous driving system which always outperforms human drivers.
- To provide a platform that enables Autoware users to develop the autonomous driving system achieving "100% accuracy" or "error-free recognition".

**Non-goals:**

- To develop the perception component architecture specialized for specific / limited ODDs.
- To achieve the complete functionality and capability.
- To outperform the recognition capability of human drivers.
- To achieve "100% accuracy" or "error-free recognition".

## High-level architecture

This diagram describes the high-level architecture of the Perception Component.

![overall-perception-architecture](image/high-level-perception-diagram.drawio.svg)

The Perception Component consists of the following sub-components:

- **Object Recognition**: Recognizes dynamic objects surrounding the ego vehicle in the current frame and predicts their future trajectories.
- **Obstacle Segmentation**: Identifies point clouds originating from obstacles(not only dynamic objects but also static obstacles that should be avoided, such as stationary obstacles) that the ego vehicle should avoid.
- **Occupancy Grid Map**: Detects blind spots (areas where no information is available and where dynamic objects may jump out).
- **Traffic Light Recognition**: Recognizes the colors of traffic lights and the directions of arrow signals.

## Component interface

The following describes the input/output concept between Perception Component and other components. See [the Perception Component Interface](../../autoware-interfaces/components/perception.md) page for the current implementation.

### Input to the Perception Component

- **From Sensing**: This input should provide real-time information about the environment.
  - Camera Image: Image data obtained from the camera.
  - Point Cloud: Point Cloud data obtained from LiDAR.
  - Radar Object: Object data obtained from radar.
- **From Localization**: This input should provide real-time information about the ego vehicle.
  - Vehicle motion information: Includes the ego vehicle's position.
- **From Map**: This input should provide real-time information about the static information about the environment.
  - Vector Map: Contains all static information about the environment, including lane aria information.
  - Point Cloud Map: Contains static point cloud maps, which should not include information about the dynamic objects.
- **From API**:
  - V2X information: The information from V2X modules. For example, the information from traffic signals.

### Output from the Perception Component

- **To Planning**
  - Dynamic Objects: Provides real-time information about objects that cannot be known in advance, such as pedestrians and other vehicles.
  - Obstacle Segmentation: Supplies real-time information about the location of obstacles, which is more primitive than Detected Object.
  - Occupancy Grid Map: Offers real-time information about the presence of occluded area information.
  - Traffic Light Recognition result: Provides the current state of each traffic light in real time.

## How to add new modules (WIP)

As mentioned in the goal session, this perception module is designed to be extensible by third-party components. For specific instructions on how to add new modules and expand its functionality, please refer to the provided documentation or guidelines (WIP).

## Supported Functions

| Feature                      | Description                                                                                                                                                                                                                                                       | Requirements                                                    |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| LiDAR DNN based 3D detector  | This module takes point clouds as input and detects objects such as vehicles, trucks, buses, pedestrians, and bicycles.                                                                                                                                           | - Point Clouds                                                  |
| Camera DNN based 2D detector | This module takes camera images as input and detects objects such as vehicles, trucks, buses, pedestrians, and bicycles in the two-dimensional image space. It detects objects within image coordinates and providing 3D coordinate information is not mandatory. | - Camera Images                                                 |
| LiDAR Clustering             | This module performs clustering of point clouds and shape estimation to achieve object detection without labels.                                                                                                                                                  | - Point Clouds                                                  |
| Semi-rule based detector     | This module detects objects using information from both images and point clouds, and it consists of two components: LiDAR Clustering and Camera DNN based 2D detector.                                                                                            | - Output from Camera DNN based 2D detector and LiDAR Clustering |
| Object Merger                | This module integrates results from various detectors.                                                                                                                                                                                                            | - Detected Objects                                              |
| Interpolator                 | This module stabilizes the object detection results by maintaining long-term detection results using Tracking results.                                                                                                                                            | - Detected Objects <br> - Tracked Objects                       |
| Tracking                     | This module gives ID and estimate velocity to the detection results.                                                                                                                                                                                              | - Detected Objects                                              |
| Prediction                   | This module predicts the future paths (and their probabilities) of dynamic objects according to the shape of the map and the surrounding environment.                                                                                                             | - Tracked Objects <br> - Vector Map                             |
| Obstacle Segmentation        | This module identifies point clouds originating from obstacles that the ego vehicle should avoid.                                                                                                                                                                 | - Point Clouds <br> - Point Cloud Map                           |
| Occupancy Grid Map           | This module detects blind spots (areas where no information is available and where dynamic objects may jump out).                                                                                                                                                 | - Point Clouds <br> - Point Cloud Map                           |
| Traffic Light Recognition    | This module detects the position and state of traffic signals.                                                                                                                                                                                                    | - Camera Images <br> - Vector Map                               |

## Reference Implementation

When Autoware is launched, the default parameters are loaded, and the Reference Implementation is started. For more details, please refer to [the Reference Implementation](./reference_implementation.md).
