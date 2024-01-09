---
タイトル: /api/operation_mode/enable_autoware_control
ステータス: v1.0.0
手法: function call
型:
  name: autoware_adapi_v1_msgs/srv/ChangeOperationMode
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Autowareによる車両制御を有効にします。
詳細については[動作モード](../../../features/operation_mode.md)を参照してください。
車両がソフトウェアによるモード変更をサポートしていない場合、このAPIは失敗します。
{% endblock %}
