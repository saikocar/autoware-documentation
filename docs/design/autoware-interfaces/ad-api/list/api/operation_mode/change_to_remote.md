---
タイトル: /api/operation_mode/change_to_remote
型: v1.0.0
手法: function call
型:
  name: autoware_adapi_v1_msgs/srv/ChangeOperationMode
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
動作モードをリモートに変更します。
詳細については[動作モード](../../../features/operation_mode.md)を参照してください。
{% endblock %}
