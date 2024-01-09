---
タイトル: /api/vehicle/doors/command
ステータス: リリースされていません
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/SetDoorCommand
  req:
    - name: doors.index
      text: The index of the target door.
    - name: doors.command
      text: The command for the target door.
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
ドアの指令を設定します。このAPIは車両がソフトウェアによるドアの制御をサポートしている場合に限り有効です。
{% endblock %}
