タイトル	状態	方法	タイプ
/api/計画/協力/get_policies
解放されていない
関数呼び出し
名前	レス
autoware_adapi_v1_msgs/srv/GetCooperationPolicies
名前	文章
状態
対応状況
名前	文章
ポリシー.動作
ターゲットの動作のタイプ。
名前	文章
ポリシー.シーケンス
ターゲットシーケンスのタイプ。
名前	文章
ポリシー.ポリシー
協力ポリシーのタイプ。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} オペレーターの決定が未定の場合に代わりに使用されるデフォルトの決定を取得します。詳細については、連携を参照してください。{% エンドブロック %}
---
title: /api/planning/cooperation/get_policies
status: not released
method: function call
type:
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
Get the default decision that is used instead when the operator's decision is undecided.
For details, see the [cooperation](../../../../features/cooperation.md).
{% endblock %}
