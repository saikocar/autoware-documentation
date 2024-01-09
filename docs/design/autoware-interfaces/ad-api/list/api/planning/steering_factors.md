---
タイトル: /api/planning/steering_factors
ステータス: リリースされていません
手法: リアルタイムストリーム
型:
  name: autoware_adapi_v1_msgs/msg/SteeringFactorArray
  msg:
    - name: factors.pose
      text: The base link pose related to the steering factor.
    - name: factors.distance
      text: The distance from the base link to the above pose.
    - name: factors.direction
      text: The direction of the steering factor.
    - name: factors.status
      text: The status of the steering factor.
    - name: factors.behavior
      text: The behavior type of the steering factor.
    - name: factors.sequence
      text: The sequence type of the steering factor.
    - name: factors.detail
      text: The additional information of the steering factor.
    - name: factors.cooperation
      text: The cooperation status if the module supports.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
ステアリング係数を距離の昇順で取得します。
詳細については[計画要素](../../../features/planning-factors.md)を参照してください。
{% endblock %}
