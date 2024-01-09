---
タイトル: /api/routing/state
ステータス: v1.0.0
手法: 通知
type:
  name: autoware_adapi_v1_msgs/msg/RouteState
  msg:
    - name: state
      text: A value of the route state.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
経路状態を取得します。
詳細については[ルーティング](../../../features/routing.md)を参照してください。
{% endblock %}
