バスサービスのユーザーストーリー
概要
このユーザー ストーリーは、指定された停留所を巡るバス サービスです。

シナリオ
ステップ	手術	使用事例
1	自動運転システムを起動します。	起動と終了
2	車をガレージから待機位置まで運転します。	動作モードを変更する
3	自律制御を有効にします。	動作モードを変更する
4	次のバス停まで車を運転します。	指定された位置まで運転します
5	車両に乗り降りします。	乗って降りる
6	終点のバス停でない場合は、手順 4 に戻ります。	
7	車両を待機位置まで運転します。	指定された位置まで運転します
8	車両を待機位置からガレージまで運転します。	動作モードを変更する
9	自動運転システムを停止します。	起動と終了
# User story of bus service

## Overview

This user story is a bus service that goes around the designated stops.

## Scenario

| Step | Operation                                                  | Use Case                                                                      |
| ---- | ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1    | Startup the autonomous driving system.                     | [Launch and terminate](../use-cases/launch-terminate.md)                      |
| 2    | Drive the vehicle from the garage to the waiting position. | [Change the operation mode](../use-cases/change-operation-mode.md)            |
| 3    | Enable autonomous control.                                 | [Change the operation mode](../use-cases/change-operation-mode.md)            |
| 4    | Drive the vehicle to the next bus stop.                    | [Drive to the designated position](../use-cases/drive-designated-position.md) |
| 5    | Get on and off the vehicle.                                | [Get on and get off](../use-cases/get-on-off.md)                              |
| 6    | Return to step 4 unless it's the last bus stop.            |                                                                               |
| 7    | Drive the vehicle to the waiting position.                 | [Drive to the designated position](../use-cases/drive-designated-position.md) |
| 8    | Drive the vehicle from the waiting position to the garage. | [Change the operation mode](../use-cases/change-operation-mode.md)            |
| 9    | Shutdown the autonomous driving system.                    | [Launch and terminate](../use-cases/launch-terminate.md)                      |
