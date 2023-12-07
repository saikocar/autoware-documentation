タイトル	状態	方法	タイプ
/api/車両/ステータス
解放されていない
通知
名前	メッセージ
autoware_adapi_v1_msgs/msg/VehicleStatus
名前	文章
装備
ギアのステータス。
名前	文章
ターンインジケーター
方向指示器の状態は、左または右のどちらかのみが有効になります。
名前	文章
ハザードライト
ハザードランプの状態。
名前	文章
ステアリングタイヤ角度
車両の現在のタイヤ角度 (ラジアン単位)。
名前	文章
エネルギーの割合
バッテリーの割合または燃料の割合は車両によって異なります。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} 車両の状態情報を公開します。{% エンドブロック %}
---
title: /api/vehicle/status
status: not released
method: notification
type:
  name: autoware_adapi_v1_msgs/msg/VehicleStatus
  msg:
    - name: gear
      text: Gear status.
    - name: turn_indicators
      text: Turn indicators status, only either left or right will be enabled.
    - name: hazard_lights
      text: Hazard lights status.
    - name: steering_tire_angle
      text: Vehicle current tire angle in radian.
    - name: energy_percentage
      text: Battery percentage or fuel percentage, it will depends on the vehicle.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Publish vehicle state information.
{% endblock %}
