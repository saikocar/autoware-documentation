タイトル	状態	方法	タイプ
/api/localization/initialization_state
v1.0.0
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/LocalizationInitializationState
名前	文章
州
ローカリゼーションの初期化状態の値。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} ローカリゼーションの初期化状態を取得します。詳細については、「ローカリゼーション」を参照してください。{% エンドブロック %}
---
title: /api/localization/initialization_state
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/LocalizationInitializationState
  msg:
    - name: state
      text: A value of the localization initialization state.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the initialization state of localization.
For details, see the [localization](../../../features/localization.md).
{% endblock %}
