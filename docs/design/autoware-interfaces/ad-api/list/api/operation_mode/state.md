---
タイトル: /api/operation_mode/state
ステータス: v1.0.0
手法: 通知
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
動作モードの状態を取得します。
詳細については[動作モード](../../../features/operation_mode.md)を参照してください。
{% endblock %}
