タイトル	状態	方法	タイプ
/api/ルーティング/ルート
v1.0.0
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/Route
名前	文章
ヘッダ
ポーズ変換用のヘッダー
名前	文章
データ
レーンレット形式のルート
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% block description %} レーンレット形式のウェイポイント セグメントを含むルートを取得します。ルートが設定されていない場合は空です。{% エンドブロック %}
---
title: /api/routing/route
status: v1.0.0
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/Route
  msg:
    - name: header
      text: header for pose transformation
    - name: data
      text: The route in lanelet format
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the route with the waypoint segments in lanelet format. It is empty if route is not set.
{% endblock %}
