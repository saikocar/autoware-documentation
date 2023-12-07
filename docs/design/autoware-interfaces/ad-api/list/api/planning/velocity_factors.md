タイトル	状態	方法	タイプ
/api/計画/velocity_factors
解放されていない
リアルタイムストリーム
名前	メッセージ
autoware_adapi_v1_msgs/msg/VelocityFactorArray
名前	文章
要因のポーズ
速度係数に関連するベース リンクのポーズ。
名前	文章
係数.距離
ベースリンクから上記のポーズまでの距離。
名前	文章
要因.ステータス
速度係数のステータス。
名前	文章
要因.行動
速度係数の動作タイプ。
名前	文章
因子.シーケンス
速度係数のシーケンス タイプ。
名前	文章
要因の詳細
速度係数の追加情報。
名前	文章
要因.協力
モジュールがサポートしている場合の連携ステータス。
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} 距離の昇順に並べ替えられた速度係数を取得します。詳細については、計画要素を参照してください。{% エンドブロック %}
---
title: /api/planning/velocity_factors
status: not released
method: realtime stream
type:
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
Get the velocity factors, sorted in ascending order of distance.
For details, see the [planning factors](../../../features/planning-factors.md).
{% endblock %}
