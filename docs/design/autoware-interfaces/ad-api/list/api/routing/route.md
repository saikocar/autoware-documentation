---
タイトル: /api/routing/route
ステータス: v1.0.0
手法: 通知
型:
  name: autoware_adapi_v1_msgs/msg/Route
  msg:
    - name: header
      text: header for pose transformation
    - name: data
      text: The route in lanelet format
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
レーンレット形式のウェイポイントセグメントを含む経路を取得します。経路が設定されていない場合は空です。
{% endblock %}
