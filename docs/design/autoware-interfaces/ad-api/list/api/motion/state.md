---
タイトル: /api/motion/state
ステータス: リリースされていません
手法: 通知
型:
  name: autoware_adapi_v1_msgs/msg/MotionState
  msg:
    - name: state
      text: A value of the motion state.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
動作状態を取得します。
詳細については[動作状態](../../../features/motion.md)を参照してください。
{% endblock %}
