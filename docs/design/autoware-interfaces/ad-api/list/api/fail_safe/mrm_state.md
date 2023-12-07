タイトル	状態	方法	タイプ
/api/fail_safe/mrm_state
解放されていない
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/MrmState
名前	文章
州
MRM の動作状態。
名前	文章
行動
現在選択されている MRM の動作。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} MRM 状態を取得します。詳細については、「フェールセーフ」を参照してください。{% エンドブロック %}
---
title: /api/fail_safe/mrm_state
status: not released
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/MrmState
  msg:
    - name: state
      text: The state of MRM operation.
    - name: behavior
      text: The currently selected behavior of MRM.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the MRM state.
For details, see the [fail-safe](../../../features/fail-safe.md).
{% endblock %}
