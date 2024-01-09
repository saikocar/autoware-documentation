---
タイトル: /api/localization/initialize
状態: v1.0.0
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/InitializeLocalization
  req:
    - name: pose
      text: A global pose as the initial guess. If omitted, the GNSS pose will be used.
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
位置推定の初期化を要求します。
詳細については[localization](../../../features/localization.md)を参照してください。
{% endblock %}
