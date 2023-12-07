タイトル	状態	方法	タイプ
/api/知覚/オブジェクト
v1.0.0
リアルタイムストリーム
名前	メッセージ
autoware_adapi_v1_msgs/msg/DynamicObjectArray
名前	文章
オブジェクト.id
各オブジェクトの UUID
名前	文章
オブジェクトの存在確率
物体が出る確率
名前	文章
オブジェクトの分類
認識されたオブジェクトのタイプと信頼度
名前	文章
オブジェクト.キネマティクス
オブジェクトのポーズ、ツイスト、加速度、およびpredicted_pa​​thsで構成されます。
名前	文章
オブジェクトの形状
物体の形状を寸法や多角形で表現する
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% block description %} ラベル、形状、現在位置、予測されたパスを含む認識されたオブジェクトの配列を取得します 詳細については、認識 を参照してください。{% エンドブロック %}
---
title: /api/perception/objects
status: v1.0.0
method: realtime stream
type:
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
Get the recognized objects array with label, shape, current position and predicted path
For details, see the [perception](../../../features/perception.md).
{% endblock %}
