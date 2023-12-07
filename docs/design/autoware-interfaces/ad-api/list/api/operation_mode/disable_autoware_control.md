タイトル	状態	方法	タイプ
/api/operation_mode/disable_autoware_control
v1.0.0
関数呼び出し
名前	レス
autoware_adapi_v1_msgs/srv/ChangeOperationMode
名前	文章
状態
対応状況
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} Autoware による車両制御を無効にします。詳細は動作モードを参照してください。車両がソフトウェアによるモード変更をサポートしていない場合、この API は失敗します。{% エンドブロック %}
---
title: /api/operation_mode/disable_autoware_control
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/ChangeOperationMode
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Disable vehicle control by Autoware.
For details, see the [operation mode](../../../features/operation_mode.md).
This API fails if the vehicle does not support mode change by software.
{% endblock %}
