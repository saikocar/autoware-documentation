# 位置推定

## 関連するAPI

- {{ link_ad_api('/api/localization/initialization_state') }}
- {{ link_ad_api('/api/localization/initialize') }}

## 説明

このAPIは位置推定の初期化を管理します。Autowareでは、位置推定の最初の推測としてグローバルの姿勢が必要です。

## 状態

![位置推定初期化状態](./localization/state.drawio.svg)

| 状態         | 説明                                                                      |
| ------------- | -------------------------------------------------------------------------------- |
| UNINITIALIZED | 位置推定が初期化されていません。最初の推測としてグローバルの姿勢を待っています。 |
| INITIALIZING  | 位置推定を初期化しています。                                                    |
| INITIALIZED   | 位置推定が初期化されます。必要に応じて、再度初期化を要求できます。 |
