# Autoware AD API

## 概要

Autoware AD APIは、自動運転システムの外部から車両を操作するためのインターフェースです。
[Autowareの全体的なインターフェイス設計については、こちらを参照してください。](../index.md)

## ユーザーストーリー

ユーザーストーリーは、AD APIが想定するサービスシナリオです。AD APIはこれらのシナリオに基づいて設計されています。
各シナリオは、後述するユースケースの組み合わせによって実現されます。
カバーできないシナリオがある場合は、ユーザーストーリーの追加についてご相談ください。

- [バスサービス](./stories/bus-service.md)
- [タクシーサービス](./stories/taxi-service.md)

## ユースケース

ユースケースは、ユーザーストーリーから派生し、一般的に設計された部分的なシナリオです。
サービスプロバイダーは、これらのユースケースを組み合わせてユーザーストーリーを定義し、AD APIを独自のシナリオに適用できるかどうかを確認できます。

- [起動と終了](./use-cases/launch-terminate.md)
- [ポーズを初期化する](./use-cases/initialize-pose.md)
- [動作モードを変更する](./use-cases/change-operation-mode.md)
- [指定された位置まで運転します](./use-cases/drive-designated-position.md)
- [乗降](./use-cases/get-on-off.md)
- [車両監視](./use-cases/vehicle-monitoring.md)
- [車両の運行](./use-cases/vehicle-operation.md)

## 機能

- [インターフェース](./features/interface.md)
- [動作モード](./features/operation_mode.md)
- [ルーティング](./features/routing.md)
- [位置推定](./features/localization.md)
- [動作](./features/motion.md)
- [計画](./features/planning-factors.md)
- [知覚](./features/perception.md)
- [フェイルセーフ](./features/fail-safe.md)
- [車両状態](./features/vehicle-status.md)
- [車両ドア](./features/vehicle-doors.md)
- [連携](./features/cooperation.md)
