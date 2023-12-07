タイトル	状態	方法	タイプ
/api/ルーティング/clear_route
v1.0.0
関数呼び出し
名前	レス
autoware_adapi_v1_msgs/srv/ClearRoute
名前	文章
状態
対応状況
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% block description %} ルートをクリアします。{% エンドブロック %}
---
title: /api/routing/clear_route
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ClearRoute
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Clear the route.
{% endblock %}
