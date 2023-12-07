動作モードを変更する
関連するAPI
動作モード
順序
ソフトウェアスイッチでモードを切り替えます。

{% include 'design/autoware-interfaces/ad-api/use-cases/sequence/operation-mode-software.plantuml' %}
ハードウェアスイッチでモードを変更します。

{% include 'design/autoware-interfaces/ad-api/use-cases/sequence/operation-mode-hardware.plantuml' %}

# Change the operation mode

## Related API

- Operation mode

## Sequence

- Change the mode with software switch.

  ```plantuml
  {% include 'design/autoware-interfaces/ad-api/use-cases/sequence/operation-mode-software.plantuml' %}
  ```

- Change the mode with hardware switch.

  ```plantuml
  {% include 'design/autoware-interfaces/ad-api/use-cases/sequence/operation-mode-hardware.plantuml' %}
  ```
