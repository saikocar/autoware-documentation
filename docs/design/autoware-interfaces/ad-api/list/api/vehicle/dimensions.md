---
タイトル: /api/vehicle/dimensions
ステータス: リリースされていません
手法: 関数呼び出し
型:
  name: autoware_adapi_v1_msgs/srv/GetVehicleDimensions
  res:
    - name: status
      text: response status
    - name: dimensions
      text: vehicle dimensions
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
車両の寸法を取得します。各値の定義については、[こちら](../../../../components/vehicle-dimensions.md)を参照してください。
{% endblock %}
