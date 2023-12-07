タイトル	状態	方法	タイプ
/api/インターフェイス/バージョン
v1.0.0
関数呼び出し
名前	レス
autoware_adapi_version_msgs/srv/InterfaceVersion
名前	文章
選考科目
メジャーバージョン
名前	文章
マイナー
マイナーバージョン
名前	文章
パッチ
パッチバージョン
{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %} {% ブロックの説明 %} インターフェイスのバージョンを取得します。バージョンはセマンティック バージョニングに従います。{% エンドブロック %}


---
title: /api/interface/version
status: v1.0.0
method: function call
type:
  name: autoware_adapi_version_msgs/srv/InterfaceVersion
  res:
    - name: major
      text: major version
    - name: minor
      text: minor version
    - name: patch
      text: patch version
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
Get the interface version. The version follows Semantic Versioning.
{% endblock %}
