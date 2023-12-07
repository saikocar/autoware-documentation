タイトル	状態	方法	タイプ
/api/モーション/状態
解放されていない
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/MotionState
名前	文章
州
動作状態の値。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} モーション状態を取得します。詳細については、動作状態を参照してください。{% エンドブロック %}
---
title: /api/motion/state
status: not released
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/MotionState
  msg:
    - name: state
      text: A value of the motion state.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the motion state.
For details, see the [motion state](../../../features/motion.md).
{% endblock %}
