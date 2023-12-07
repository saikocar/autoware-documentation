オートウェア AD API
概要
Autoware AD APIは、自動運転システムの外部から車両を操作するためのインターフェースです。 Autoware の全体的なインターフェイス設計については、こちらを参照してください。

ユーザーストーリー
ユーザーストーリーは、AD APIが想定するサービスシナリオです。AD API はこれらのシナリオに基づいて設計されています。各シナリオは、後述するユースケースの組み合わせによって実現されます。カバーできないシナリオがある場合は、ユーザー ストーリーの追加についてご相談ください。

バスサービス
タクシーサービス
ユースケース
ユースケースは、ユーザーストーリーから派生し、一般的に設計された部分的なシナリオです。サービス プロバイダーは、これらのユース ケースを組み合わせてユーザー ストーリーを定義し、AD API を独自のシナリオに適用できるかどうかを確認できます。

起動と終了
ポーズを初期化する
動作モードを変更する
指定された位置まで運転します
乗って降りる
車両監視
車両の運行
特徴
インターフェース
動作モード
ルーティング
ローカリゼーション
モーション
企画
感知
フェイルセーフ
車両状態
車両ドア
協力
# Autoware AD API

## Overview

Autoware AD API is the interface for operating the vehicle from outside the autonomous driving system.
[See here for the overall interface design of Autoware.](../index.md)

## User stories

The user stories are service scenarios that AD API assumes. AD API is designed based on these scenarios.
Each scenario is realized by a combination of use cases described later.
If there are scenarios that cannot be covered, please discuss adding a user story.

- [Bus service](./stories/bus-service.md)
- [Taxi service](./stories/taxi-service.md)

## Use cases

Use cases are partial scenarios derived from the user story and generically designed.
Service providers can combine these use cases to define user stories and check if AD API can be applied to their own scenarios.

- [Launch and terminate](./use-cases/launch-terminate.md)
- [Initialize the pose](./use-cases/initialize-pose.md)
- [Change the operation mode](./use-cases/change-operation-mode.md)
- [Drive to the designated position](./use-cases/drive-designated-position.md)
- [Get on and get off](./use-cases/get-on-off.md)
- [Vehicle monitoring](./use-cases/vehicle-monitoring.md)
- [Vehicle operation](./use-cases/vehicle-operation.md)

## Features

- [Interface](./features/interface.md)
- [Operation Mode](./features/operation_mode.md)
- [Routing](./features/routing.md)
- [Localization](./features/localization.md)
- [Motion](./features/motion.md)
- [Planning](./features/planning-factors.md)
- [Perception](./features/perception.md)
- [Fail-safe](./features/fail-safe.md)
- [Vehicle status](./features/vehicle-status.md)
- [Vehicle doors](./features/vehicle-doors.md)
- [Cooperation](./features/cooperation.md)
