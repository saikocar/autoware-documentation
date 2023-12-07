タイトル	状態	方法	タイプ
/api/planning/steering_factors
解放されていない
リアルタイムストリーム
名前	メッセージ
autoware_adapi_v1_msgs/msg/SteeringFactorArray
名前	文章
要因のポーズ
ステアリング係数に関連するベース リンクのポーズ。
名前	文章
係数.距離
ベースリンクから上記のポーズまでの距離。
名前	文章
要因.方向
ステアリング係数の方向。
名前	文章
要因.ステータス
ステアリング係数のステータス。
名前	文章
要因.行動
ステアリング係数の動作タイプ。
名前	文章
因子.シーケンス
ステアリング係数のシーケンス タイプ。
名前	文章
要因の詳細
ステアリング係数の追加情報。
名前	文章
要因.協力
モジュールがサポートしている場合の連携ステータス。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} ステアリング係数を距離の昇順で取得します。詳細については、計画要素を参照してください。{% エンドブロック %}
---
title: /api/planning/steering_factors
status: not released
method: realtime stream
type:
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
Get the steering factors, sorted in ascending order of distance.
For details, see the [planning factors](../../../features/planning-factors.md).
{% endblock %}
