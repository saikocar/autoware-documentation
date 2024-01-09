---
タイトル: /api/localization/initialization_state
ステータス: v1.0.0
手法: 通知
型:
  name: autoware_adapi_v1_msgs/msg/LocalizationInitializationState
  msg:
    - name: state
      text: A value of the localization initialization state.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
位置推定の初期状態を取得します。
詳細については[localization](../../../features/localization.md)を参照してください。
{% endblock %}
