タイトル	状態	方法	タイプ
/api/planning/cooperation/set_commands
解放されていない
関数呼び出し
名前	要求	レス
autoware_adapi_v1_msgs/srv/SetCooperationCommands
名前	文章
コマンド.uuid
連携状態のIDです。
名前	文章
コマンド.協力者
オペレーターの判断。
名前	文章
状態
対応状況
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% block description %} 連携に関するオペレータの決定を設定します。詳細については、連携を参照してください。{% エンドブロック %}
---
title: /api/planning/cooperation/set_commands
status: not released
method: function call
type:
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
Set the operator's decision for cooperation.
For details, see the [cooperation](../../../../features/cooperation.md).
{% endblock %}
