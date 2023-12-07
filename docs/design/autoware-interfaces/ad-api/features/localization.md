ローカリゼーション
関連するAPI
{{ link_ad_api('/api/localization/initialization_state') }}
{{ link_ad_api('/api/localization/initialize') }}
説明
この API はローカリゼーションの初期化を管理します。Autoware では、ローカリゼーションの最初の推測としてグローバル ポーズが必要です。

州
ローカリゼーション初期化状態

州	説明
未初期化	ローカリゼーションが初期化されていません。最初の推測としてグローバル ポーズを待っています。
初期化中	ローカリゼーションを初期化しています。
初期化されました	ローカリゼーションが初期化されます。必要に応じて、再度初期化を要求できます。
# Localization

## Related API

- {{ link_ad_api('/api/localization/initialization_state') }}
- {{ link_ad_api('/api/localization/initialize') }}

## Description

This API manages the initialization of localization. Autoware requires a global pose as the initial guess for localization.

## States

![localization-initialization-state](./localization/state.drawio.svg)

| State         | Description                                                                      |
| ------------- | -------------------------------------------------------------------------------- |
| UNINITIALIZED | Localization is not initialized. Waiting for a global pose as the initial guess. |
| INITIALIZING  | Localization is initializing.                                                    |
| INITIALIZED   | Localization is initialized. Initialization can be requested again if necessary. |
