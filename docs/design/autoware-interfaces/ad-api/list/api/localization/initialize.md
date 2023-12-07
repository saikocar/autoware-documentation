タイトル	状態	方法	タイプ
/api/ローカリゼーション/初期化
v1.0.0
関数呼び出し
名前	要求	レス
autoware_adapi_v1_msgs/srv/InitializeLocalization
名前	文章
ポーズ
最初の推測としてはグローバル ポーズ。省略した場合は、GNSS ポーズが使用されます。
名前	文章
状態
対応状況
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} ローカリゼーションの初期化を要求します。詳細については、「ローカリゼーション」を参照してください。{% エンドブロック %}
---
title: /api/localization/initialize
status: v1.0.0
method: function call
type:
  name: autoware_adapi_v1_msgs/srv/InitializeLocalization
  req:
    - name: pose
      text: A global pose as the initial guess. If omitted, the GNSS pose will be used.
  res:
    - name: status
      text: response status
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Request to initialize localization.
For details, see the [localization](../../../features/localization.md).
{% endblock %}
