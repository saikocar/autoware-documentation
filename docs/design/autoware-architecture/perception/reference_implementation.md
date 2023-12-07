知覚コンポーネントのリファレンス実装設計
この文書の目的
このドキュメントでは、リファレンス実装の詳細な設計の概要を説明します。これにより、開発者とユーザーは、Perception コンポーネントで現在何が利用できるのか、その機能を利用、拡張、または追加する方法を理解できるようになります。

建築
この図は、リファレンス実装のアーキテクチャを示しています。

全体的な認識のアーキテクチャ

Perception コンポーネントは次のサブコンポーネントで構成されます。

障害物セグメンテーション: 自車両が回避すべき障害物 (動的オブジェクトだけでなく、静止障害物などの回避すべき静的障害物も含む) に由来する点群を識別します。たとえば、建設用コーンはこのモジュールを使用して認識されます。
占有グリッド マップ: 死角 (情報が入手できず、動的オブジェクトが飛び出す可能性のある領域) を検出します。
物体認識: 現在のフレーム内で自車両の周囲にある動的物体を認識し、それらの将来の軌道を予測します。
検出: 車両や歩行者などの動的物体の姿勢と速度を検出します。
Detector : オブジェクト検出処理をフレームごとにトリガーします。
Interpolator : 安定した物体検出を維持します。Detector からの出力が突然利用できなくなった場合でも、Interpolator は Tracking モジュールからの出力を使用して、オブジェクトを見逃すことなく検出結果を維持します。
追跡: 複数のフレームにわたる検出結果を関連付けます。
予測: 動的オブジェクトの軌道を予測します。
信号機認識: 信号機の色と矢印信号の方向を認識します。
認識コンポーネントの内部インターフェイス
障害物のセグメンテーションから物体認識へ
点群: 現在のフレームで観察された点群で、地面と外れ値が削除されています。
障害物セグメンテーションから占有グリッド マップへ
地面フィルターされた点群: 現在のフレームで観察され、地面が削除された点群。
障害物セグメンテーションへの占有グリッド マップ
占有グリッド マップ: これは外れ値をフィルタリングするために使用されます。
# Perception Component Reference Implementation Design

## Purpose of this document

This document outlines detailed design of the reference imprementations. This allows developers and users to understand what is currently available with the Perception Component, how to utilize, expand, or add to its features.

## Architecture

This diagram describes the architecture of the reference implementation.

![overall-perception-architecture](image/reference-implementaion-perception-diagram.drawio.svg)

The Perception component consists of the following sub-components:

- **Obstacle Segmentation**: Identifies point clouds originating from obstacles(not only dynamic objects but also static obstacles that should be avoided, such as stationary obstacles) that the ego vehicle should avoid. For example, construction cones are recognized using this module.
- **Occupancy Grid Map**: Detects blind spots (areas where no information is available and where dynamic objects may jump out).
- **Object Recognition**: Recognizes dynamic objects surrounding the ego vehicle in the current frame and predicts their future trajectories.
  - **Detection**: Detects the pose and velocity of dynamic objects such as vehicles and pedestrians.
    - **Detector**: Triggers object detection processing frame by frame.
    - **Interpolator**: Maintains stable object detection. Even if the output from Detector suddenly becomes unavailable, Interpolator uses the output from the Tracking module to maintain the detection results without missing any objects.
  - **Tracking**: Associates detected results across multiple frames.
  - **Prediction**: Predicts trajectories of dynamic objects.
- **Traffic Light Recognition**: Recognizes the colors of traffic lights and the directions of arrow signals.

### Internal interface in the perception component

- **Obstacle Segmentation to Object Recognition**
  - Point Cloud: A Point Cloud observed in the current frame, where the ground and outliers are removed.
- **Obstacle Segmentation to Occupancy Grid Map**
  - Ground filtered Point Cloud: A Point Cloud observed in the current frame, where the ground is removed.
- **Occupancy Grid Map to Obstacle Segmentation**
  - Occupancy Grid Map: This is used for filtering outlier.