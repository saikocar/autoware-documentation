---
タイトル: /api/fail_safe/mrm_state
ステータス: リリースされていません
手法: 告知
型:
  name: autoware_adapi_v1_msgs/msg/MrmState
  msg:
    - name: state
      text: MRMの動作状態
    - name: behavior
      text: 現在選択されているMRMの動作
---

{% extends 'design/autoware-interfaces/templates/autoware-interface.jinja2' %}
{% block description %}
MRMの状態を取得します。
詳細は[フェイルセーフ](../../../features/fail-safe.md)を参照してください。
{% endblock %}
