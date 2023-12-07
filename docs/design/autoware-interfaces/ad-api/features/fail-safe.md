フェイルセーフ
関連するAPI
{{ link_ad_api('/api/fail_safe/mrm_state') }}
説明
車両の異常に関する挙動を管理するAPIです。これは、介入要求 (RTI)、最小リスク操作 (MRM)、および最小リスク条件 (MRC) の状態を提供します。Autoware には以下に示すように、通常動作時のコマンドと異常動作時のコマンドを切り替えるゲートがあります。安全のため、Autoware は異常を検出すると MRM に動作を切り替えます。状況に応じて求められる動作が異なるため、MRMは通常のモジュール内の特定のモード、または独立したモジュールとしてさまざまな場所に実装されます。フェールセーフモジュールは異常に応じてMRMの動作を選択し、ゲート出力をそのコマンドに切り替えます。

フェイルセーフアーキテクチャ

州
MRM 状態は、MRM が動作しているかどうかを示します。この状態は成功または失敗も示します。通常、MRM は失敗すると別の動作に切り替わります。

mrm-状態

州	説明
なし	MRM が動作していません。
オペレーティング	異常を検知したためMRMが動作しています。
成功しました	MRMは成功しました。車両は安全な状態にあります。
失敗した	MRM が失敗しました。車両は依然として危険な状態にある。
行動
MRM の動作間には依存関係があります。たとえば、快適な停止から緊急停止に切り替わりますが、その逆は切り替わりません。これはサービスに依存します。Autoware は、デフォルトで次のトランジションをサポートします。

mrm の動作

州	説明
なし	MRM が動作していないか、動作しているが特別な動作は必要ありません。
快適_停止	心地よい減速でクルマはすぐに止まります。
緊急停止	車両は可能な限り減速して直ちに停止します。
# Fail-safe

## Related API

- {{ link_ad_api('/api/fail_safe/mrm_state') }}

## Description

This API manages the behavior related to the abnormality of the vehicle.
It provides the state of Request to Intervene (RTI), Minimal Risk Maneuver (MRM) and Minimal Risk Condition (MRC).
As shown below, Autoware has the gate to switch between the command during normal operation and the command during abnormal operation.
For safety, Autoware switches the operation to MRM when an abnormality is detected.
Since the required behavior differs depending on the situation, MRM is implemented in various places as a specific mode in a normal module or as an independent module.
The fail-safe module selects the behavior of MRM according to the abnormality and switches the gate output to that command.

![fail-safe-architecture](./fail-safe/architecture.drawio.svg)

## States

The MRM state indicates whether MRM is operating. This state also provides success or failure.
Generally, MRM will switch to another behavior if it fails.

![mrm-state](./fail-safe/mrm-state.drawio.svg)

| State     | Description                                                |
| --------- | ---------------------------------------------------------- |
| NONE      | MRM is not operating.                                      |
| OPERATING | MRM is operating because an abnormality has been detected. |
| SUCCEEDED | MRM succeeded. The vehicle is in a safe condition.         |
| FAILED    | MRM failed. The vehicle is still in an unsafe condition.   |

## Behavior

There is a dependency between MRM behaviors. For example, it switches from a comfortable stop to a emergency stop, but not the other way around.
This is service dependent. Autoware supports the following transitions by default.

![mrm-behavior](./fail-safe/mrm-behavior.drawio.svg)

| State            | Description                                                               |
| ---------------- | ------------------------------------------------------------------------- |
| NONE             | MRM is not operating or is operating but no special behavior is required. |
| COMFORTABLE_STOP | The vehicle will stop quickly with a comfortable deceleration.            |
| EMERGENCY_STOP   | The vehicle will stop immediately with as much deceleration as possible.  |
