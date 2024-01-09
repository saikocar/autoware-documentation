---
タイトル: /api/planning/velocity_factors
ステータス: リリースされていません
手法: リアルタイムストリーム
型:
  name: autoware_adapi_v1_msgs/msg/VelocityFactorArray
  msg:
    - name: factors.pose
      text: The base link pose related to the velocity factor.
    - name: factors.distance
      text: The distance from the base link to the above pose.
    - name: factors.status
      text: The status of the velocity factor.
    - name: factors.behavior
      text: The behavior type of the velocity factor.
    - name: factors.sequence
      text: The sequence type of the velocity factor.
    - name: factors.detail
      text: The additional information of the velocity factor.
    - name: factors.cooperation
      text: The cooperation status if the module supports.
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
距離の昇順に並べ替えられた速度係数を取得します。
詳細については[計画要素](../../../features/planning-factors.md)を参照してください。
{% endblock %}
