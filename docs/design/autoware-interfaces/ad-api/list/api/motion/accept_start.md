---
タイトル: /api/motion/accept_start
状態: リリースされていません
手法: function call
型:
  name: autoware_adapi_v1_msgs/srv/AcceptStart
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
車両の始動を許可します。この API は、[motion状態](../../../features/motion.md)がSTARTINGの場合に使用できます。
{% endblock %}
