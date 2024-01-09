---
タイトル: /api/planning/cooperation/set_policies
ステータス: リリースされていません
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/SetCooperationPolicies
  req:
    - name: policies.behavior
      text: The type of the target behavior.
    - name: policies.sequence
      text: The type of the target sequence.
    - name: policies.policy
      text: The type of the cooperation policy.
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
オペレータの決定が未定の場合に代わりに使用されるデフォルトの決定を設定します。
詳細については[連携](../../../../features/cooperation.md)を参照てください。
{% endblock %}
