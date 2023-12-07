タイトル	状態	方法	タイプ
/api/車両/ディメンション
解放されていない
関数呼び出し
名前	レス
autoware_adapi_v1_msgs/srv/GetVehicleDimensions
名前	文章
状態
対応状況
名前	文章
寸法
車両寸法
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} 車両の寸法を取得します。各値の定義については、こちらを参照してください。{% エンドブロック %}
---
title: /api/vehicle/dimensions
status: not released
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/GetVehicleDimensions
  res:
    - name: status
      text: response status
    - name: dimensions
      text: vehicle dimensions
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the vehicle dimensions. See [here](../../../../components/vehicle-dimensions.md) for the definition of each value.
{% endblock %}
