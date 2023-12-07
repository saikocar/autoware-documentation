タイトル	状態	方法	タイプ
/api/ルーティング/状態
v1.0.0
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/RouteState
名前	文章
州
ルート状態の値。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} ルート状態を取得します。詳細については、ルーティングを参照してください。{% エンドブロック %}
---
title: /api/routing/state
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/RouteState
  msg:
    - name: state
      text: A value of the route state.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the route state.
For details, see the [routing](../../../features/routing.md).
{% endblock %}
