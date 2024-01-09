---
タイトル: /api/interface/version
ステータス: v1.0.0
方法: 関数呼び出し
型:
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
インターフェイスのバージョンを取得します。バージョンはセマンティック バージョニングに従います。
{% endblock %}
