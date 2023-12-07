タイトル	状態	方法	タイプ
/api/ルーティング/set_route
v1.0.0
関数呼び出し
名前	要求	レス
autoware_adapi_v1_msgs/srv/SetRoute
名前	文章
ヘッダ
ポーズ変換用のヘッダー
名前	文章
ゴール
ゴールポーズ
名前	文章
セグメント
レーンレット形式のウェイポイントセグメント
名前	文章
状態
対応状況
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% block description %} レーンレット形式のウェイポイントセグメントでルートを設定します。開始ポーズが指定されていない場合は、現在のポーズが使用されます。{% エンドブロック %}
---
title: /api/routing/set_route
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/SetRoute
  req:
    - name: header
      text: header for pose transformation
    - name: goal
      text: goal pose
    - name: segments
      text: waypoint segments in lanelet format
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Set the route with the waypoint segments in lanelet format. If start pose is not specified, the current pose will be used.
{% endblock %}
