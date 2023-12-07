動作モード
関連するAPI
{{ link_ad_api('/api/operation_mode/state') }}
{{ link_ad_api('/api/operation_mode/change_to_autonomous') }}
{{ link_ad_api('/api/operation_mode/change_to_stop') }}
{{ link_ad_api('/api/operation_mode/change_to_local') }}
{{ link_ad_api('/api/operation_mode/change_to_remote') }}
{{ link_ad_api('/api/operation_mode/enable_autoware_control') }}
{{ link_ad_api('/api/operation_mode/disable_autoware_control') }}
説明
以下に示すように、Autoware は車両インターフェイスに Autoware 制御と直接制御の 2 つのモードがあることを前提としています。ダイレクト制御モードでは、ステアリングやペダルなどのデバイスを使用して車両を操作します。車両が直接制御モードをサポートしていない場合は、常に Autoware 制御モードとして扱われます。Autoware 制御モードには 4 つの動作モードがあります。

モード	説明
停止	車両は停止したままにしてください。
自律型	車両を自律的に制御します。
地元	ジョイスティックなどのデバイスを使用して、近くから車両を手動で制御します。
リモート	クラウド上のWebアプリケーションから車両を手動で制御します。
動作モードアーキテクチャ

州
Autoware制御フラグ
フラグは、is_autoware_control_enabled車両が Autoware によって制御されているかどうかを示します。ソフトウェアで制御を切り替えることができる場合は、有効化および無効化 API を使用できます。車両がモード切り替えをサポートしていない場合、またはハードウェアによって切り替えられている場合、これらの API は常に失敗します。

動作モードと変更フラグ
状態は、operation_modeAutoware 制御が有効な場合にどのコマンドが使用されるかを示します。フラグによりchange_to_*​​各モードへの遷移が可能かどうかを確認することができます。

移行フラグ
Autoware では、速度超過時の自律モードへの切り替えなどの安全性を保証できない場合があります。この状況を表すフラグがありis_in_transition、モードを変更するときにこれが true になります。モードを変更したオペレータは、このフラグが true の間、安全を確保する必要があります。モード変更が完了すると、フラグは false になります。
# Operation mode

## Related API

- {{ link_ad_api('/api/operation_mode/state') }}
- {{ link_ad_api('/api/operation_mode/change_to_autonomous') }}
- {{ link_ad_api('/api/operation_mode/change_to_stop') }}
- {{ link_ad_api('/api/operation_mode/change_to_local') }}
- {{ link_ad_api('/api/operation_mode/change_to_remote') }}
- {{ link_ad_api('/api/operation_mode/enable_autoware_control') }}
- {{ link_ad_api('/api/operation_mode/disable_autoware_control') }}

## Description

As shown below, Autoware assumes that the vehicle interface has two modes, Autoware control and direct control.
In direct control mode, the vehicle is operated using devices such as steering and pedals.
If the vehicle does not support direct control mode, it is always treated as Autoware control mode.
Autoware control mode has four operation modes.

| Mode       | Description                                                                   |
| ---------- | ----------------------------------------------------------------------------- |
| Stop       | Keep the vehicle stopped.                                                     |
| Autonomous | Autonomously control the vehicle.                                             |
| Local      | Manually control the vehicle from nearby with some device such as a joystick. |
| Remote     | Manually control the vehicle from a web application on the cloud.             |

![operation-mode-architecture](./operation_mode/architecture.drawio.svg)

## States

### Autoware control flag

The flag `is_autoware_control_enabled` indicates if the vehicle is controlled by Autoware.
The enable and disable APIs can be used if the control can be switched by software.
These APIs will always fail if the vehicle does not support mode switching or is switched by hardware.

### Operation mode and change flags

The state `operation_mode` indicates what command is used when Autoware control is enabled.
The flags `change_to_*` can be used to check if it is possible to transition to each mode.

### Transition flag

Since Autoware may not be able to guarantee safety, such as switching to autonomous mode during overspeed.
There is the flag `is_in_transition` for this situation and it will be true when changing modes.
The operator who changed the mode should ensure safety while this flag is true. The flag will be false when the mode change is complete.
