バスサービスのユーザーストーリー
概要
このユーザー ストーリーは、乗客を乗せて目的地までお送りするタクシー サービスです。

シナリオ
ステップ	手術	使用事例
1	自動運転システムを起動します。	起動と終了
2	車をガレージから待機位置まで運転します。	動作モードを変更する
3	自律制御を有効にします。	動作モードを変更する
4	車両を受け取り位置まで運転します。	指定された位置まで運転します
5	車両に乗ります。	乗って降りる
6	目的地まで車両を運転します。	指定された位置まで運転します
7	車から降りてください。	乗って降りる
8	車両を待機位置まで運転します。	指定された位置まで運転します
9	別のリクエストがある場合は、ステップ 4 に戻ります。	
10	車両を待機位置からガレージまで運転します。	動作モードを変更する
11	自動運転システムを停止します。	起動と終了
# User story of bus service

## Overview

This user story is a taxi service that picks up passengers and drives them to their destination.

## Scenario

| Step | Operation                                                  | Use Case                                                                      |
| ---- | ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1    | Startup the autonomous driving system.                     | [Launch and terminate](../use-cases/launch-terminate.md)                      |
| 2    | Drive the vehicle from the garage to the waiting position. | [Change the operation mode](../use-cases/change-operation-mode.md)            |
| 3    | Enable autonomous control.                                 | [Change the operation mode](../use-cases/change-operation-mode.md)            |
| 4    | Drive the vehicle to the position to pick up.              | [Drive to the designated position](../use-cases/drive-designated-position.md) |
| 5    | Get on the vehicle.                                        | [Get on and get off](../use-cases/get-on-off.md)                              |
| 6    | Drive the vehicle to the destination.                      | [Drive to the designated position](../use-cases/drive-designated-position.md) |
| 7    | Get off the vehicle.                                       | [Get on and get off](../use-cases/get-on-off.md)                              |
| 8    | Drive the vehicle to the waiting position.                 | [Drive to the designated position](../use-cases/drive-designated-position.md) |
| 9    | Return to step 4 if there is another request.              |                                                                               |
| 10   | Drive the vehicle from the waiting position to the garage. | [Change the operation mode](../use-cases/change-operation-mode.md)            |
| 11   | Shutdown the autonomous driving system.                    | [Launch and terminate](../use-cases/launch-terminate.md)                      |
