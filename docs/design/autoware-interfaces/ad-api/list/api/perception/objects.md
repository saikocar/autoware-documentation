---
タイトル: /api/perception/objects
状態: v1.0.0
手法: リアルタイムストリーム
型:
  name: autoware_adapi_v1_msgs/msg/DynamicObjectArray
  msg:
    - name: objects.id
      text: The UUID of each object
    - name: objects.existence_probability
      text: The probability of the object exits
    - name: objects.classification
      text: The type of the object recognized and the confidence level
    - name: objects.kinematics
      text: Consist of the object pose, twist, acceleration and the predicted_paths
    - name: objects.shape
      text: escribe the shape of the object with dimension, and polygon
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
ラベル、形状、現在位置、予測されたパスを含む認識されたオブジェクトの配列を取得します。
詳細については[認識](../../../features/perception.md)を参照してください。
{% endblock %}
