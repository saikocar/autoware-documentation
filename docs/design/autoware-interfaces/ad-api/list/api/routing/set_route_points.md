---
タイトル: /api/routing/set_route_points
ステータス: v1.0.0
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/SetRoutePoints
  req:
    - name: header
      text: header for pose transformation
    - name: goal
      text: goal pose
    - name: waypoints
      text: waypoint poses
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
ウェイポイントのポーズで経路を設定します。開始姿勢が指定されていない場合は、現在の姿勢が使用されます。
{% endblock %}
