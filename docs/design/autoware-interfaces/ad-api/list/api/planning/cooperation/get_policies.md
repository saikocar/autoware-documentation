---
タイトル: /api/planning/cooperation/get_policies
ステータス: リリースされていません
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/GetCooperationPolicies
  res:
    - name: status
      text: response status
    - name: policies.behavior
      text: The type of the target behavior.
    - name: policies.sequence
      text: The type of the target sequence.
    - name: policies.policy
      text: The type of the cooperation policy.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
オペレーターの決定が未定の場合に代わりに使用されるデフォルトの決定を取得します。
詳細については[連携](../../../../features/cooperation.md)を参照してください。
{% endblock %}
