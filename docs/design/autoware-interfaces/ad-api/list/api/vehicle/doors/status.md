---
タイトル: /api/vehicle/doors/status
ステータス: リリースされていません
手法: 通知
型:
  name: autoware_adapi_v1_msgs/msg/DoorStatusArray
  msg:
    - name: doors.status
      text: current door status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
それぞれのドアの開閉状態といったステータス。
{% endblock %}
