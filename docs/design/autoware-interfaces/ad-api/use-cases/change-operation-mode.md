# 動作モードを変更する

## 関連するAPI

- 動作モード

## 順序

- ソフトウェアスイッチでモードを切り替えます。

  ```plantuml
  {% include 'design/autoware-interfaces/ad-api/use-cases/sequence/operation-mode-software.plantuml' %}
  ```

- ハードウェアスイッチでモードを変更します。

  ```plantuml
  {% include 'design/autoware-interfaces/ad-api/use-cases/sequence/operation-mode-hardware.plantuml' %}
  ```
