---
タイトル: /api/routing/clear_route
ステータス: v1.0.0
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/ClearRoute
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
ルートをクリアします。
{% endblock %}
