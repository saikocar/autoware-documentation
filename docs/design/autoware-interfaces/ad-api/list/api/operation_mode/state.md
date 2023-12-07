タイトル	状態	方法	タイプ
/api/操作モード/状態
v1.0.0
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/OperationModeState
名前	文章
モード
Autoware 制御用に選択されたコマンド。
名前	文章
is_autoware_control_enabled
Autoware による車両制御が有効な場合は True。
名前	文章
移行中
動作モードが遷移中の場合は True。
名前	文章
is_stop_mode_available
動作モードを停止に変更できる場合は True。
名前	文章
is_autonomous_mode_available
動作モードを自律モードに変更できる場合は True。
名前	文章
is_local_mode_available
動作モードをローカルに変更できる場合は True。
名前	文章
is_remote_mode_available
動作モードをリモートに変更できる場合は True。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} 動作モードの状態を取得します。詳細は動作モードを参照してください。{% エンドブロック %}
---
title: /api/operation_mode/state
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/OperationModeState
  msg:
    - name: mode
      text: The selected command for Autoware control.
    - name: is_autoware_control_enabled
      text: True if vehicle control by Autoware is enabled.
    - name: is_in_transition
      text: True if the operation mode is in transition.
    - name: is_stop_mode_available
      text: True if the operation mode can be changed to stop.
    - name: is_autonomous_mode_available
      text: True if the operation mode can be changed to autonomous.
    - name: is_local_mode_available
      text: True if the operation mode can be changed to local.
    - name: is_remote_mode_available
      text: True if the operation mode can be changed to remote.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the operation mode state.
For details, see the [operation mode](../../../features/operation_mode.md).
{% endblock %}
