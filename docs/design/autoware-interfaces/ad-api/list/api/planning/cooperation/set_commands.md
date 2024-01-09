---
タイトル: /api/planning/cooperation/set_commands
ステータス: リリースされていません
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/SetCooperationCommands
  req:
    - name: commands.uuid
      text: The ID in the cooperation status.
    - name: commands.cooperator
      text: The operator's decision.
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
連携に関するオペレータの決定を設定します。
詳細については[連携](../../../../features/cooperation.md)を参照してください。
{% endblock %}
