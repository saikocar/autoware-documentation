# 動作モード

## 関連するAPI

- {{ link_ad_api('/api/operation_mode/state') }}
- {{ link_ad_api('/api/operation_mode/change_to_autonomous') }}
- {{ link_ad_api('/api/operation_mode/change_to_stop') }}
- {{ link_ad_api('/api/operation_mode/change_to_local') }}
- {{ link_ad_api('/api/operation_mode/change_to_remote') }}
- {{ link_ad_api('/api/operation_mode/enable_autoware_control') }}
- {{ link_ad_api('/api/operation_mode/disable_autoware_control') }}

## 説明

以下に示すように、Autowareは車両インターフェイスにAutoware制御と直接制御の2つのモードがあることを前提としています。
直接制御モードでは、ステアリングやペダルなどのデバイスを使用して車両を操作します。
車両が直接制御モードをサポートしていない場合は、常にAutoware制御モードとして扱われます。
Autoware制御モードには4つの動作モードがあります。

| モード       | 説明                                                                   |
| ---------- | ----------------------------------------------------------------------------- |
| Stop       | 車両を停止したままにします。                                                     |
| Autonomous | 車両を自律的に制御します。                                             |
| Local      | ジョイスティックなどのデバイスを使用して、近くから車両を手動で制御します。 |
| Remote     | クラウド上のWebアプリケーションから車両を手動で制御します。             |

![動作モードアーキテクチャ](./operation_mode/architecture.drawio.svg)

## 状態

### Autoware制御フラグ

`is_autoware_control_enabled`フラグは、車両がAutowareによって制御されているかどうかを示します。
ソフトウェアで制御を切り替えることができる場合は、有効化および無効化APIを使用できます。
車両がモード切り替えをサポートしていない場合、またはハードウェアによって切り替えられている場合、これらの API は常に失敗します。

### 動作モードと変更フラグ
​​
`operation_mode`状態は、Autoware 制御が有効な場合にどのコマンドが使用されるかを示します。
`change_to_*`フラグにより各モードへの遷移が可能かどうかを確認することができます。

### 移行フラグ

Autowareでは、速度超過時の自律モードへの切り替えなどの安全性を保証できない場合があります。
この状況を表す`is_in_transition`フラグがあり、モードを変更するときにこれが true になります。
モードを変更したオペレータは、このフラグがtrueの間、安全を確保する必要があります。モード変更が完了すると、フラグはfalseになります。
