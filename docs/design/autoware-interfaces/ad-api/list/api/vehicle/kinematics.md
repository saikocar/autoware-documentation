タイトル	状態	方法	タイプ
/api/vehicle/キネマティクス
解放されていない
リアルタイムストリーム
名前	メッセージ
autoware_adapi_v1_msgs/msg/VehicleKinematics
名前	文章
地理的ポーズ
車両の経度と緯度。マップがローカル座標を使用している場合は使用できません。
名前	文章
ポーズ
ベースリンクからの共分散を伴うポーズ。
名前	文章
ねじれ
共分散を伴う車両電流のねじれ。
名前	文章
アクセル
共分散を伴う車両の現在の加速度。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} 車両の運動学を公開します。{% エンドブロック %}
---
title: /api/vehicle/kinematics
status: not released
method: realtime stream
type:
  name: autoware_adapi_v1_msgs/msg/VehicleKinematics
  msg:
    - name: geographic_pose
      text: The longitude and latitude of the vehicle. If the map uses local coordinates, it will not be available.
    - name: pose
      text: The pose with covariance from the base link.
    - name: twist
      text: Vehicle current twist with covariance.
    - name: accel
      text: Vehicle current acceleration with covariance.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Publish vehicle kinematics.
{% endblock %}
