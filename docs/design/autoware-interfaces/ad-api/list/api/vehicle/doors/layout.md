---
タイトル: /api/vehicle/doors/layout
ステータス: リリースされていません
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/GetDoorLayout
  res:
    - name: status
      text: response status
    - name: doors.roles
      text: The roles of the door in the service the vehicle provides.
    - name: doors.description
      text: The description of the door for display in the interface.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
ドアの配置を取得します。これはそれぞれのドアの役割と説明の配列です。
{% endblock %}
