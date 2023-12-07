タイトル	状態	方法	タイプ
/api/motion/accept_start
解放されていない
関数呼び出し
名前	レス
autoware_adapi_v1_msgs/srv/AcceptStart
名前	文章
状態
対応状況
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} 車両の始動を受け入れます。この API は、モーション状態が STARTINGの場合に使用できます。{% エンドブロック %}
---
title: /api/motion/accept_start
status: not released
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/AcceptStart
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Accept the vehicle to start. This API can be used when the [motion state](../../../features/motion.md) is STARTING.
{% endblock %}
