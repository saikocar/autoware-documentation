車両ドア
関連するAPI
{{ link_ad_api('/api/vehicle/doors/layout') }}
{{ link_ad_api('/api/vehicle/doors/status') }}
{{ link_ad_api('/api/vehicle/doors/command') }}
説明
この機能は、車両がドア用のソフトウェア インターフェイスを提供している場合に利用できます。乗客用のユーザー インターフェイスを作成したり、バス停でのシーケンスを制御したりするために使用できます。

レイアウト
車両の各ドアには配列インデックスが割り当てられます。この割り当ては車両によって異なります。レイアウト API はこの情報を返します。description フィールドは、ユーザー インターフェイスなどに表示する文字列です。これは任意の文字列であり、アプリケーションでの処理に使用することは推奨されません。役割フィールドを使用して、乗降用のドアを確認します。以下は、レイアウト API によって返される情報の例です。

索引	説明	役割
0	右前	-
1	前方左側	乗る
2	右後	降りる
3	左奥	GET_ON、GET_OFF
状態
ステータス API は、ドアのステータスの配列を提供します。この配列の順序はレイアウト API と一致しています。

コントロール
コマンド API を使用してドアを制御します。ステータス API やレイアウト API とは異なり、配列インデックスはドアに対応しません。このコマンドには、ターゲット ドア インデックスを指定するフィールドがあります。
# Vehicle doors

## Related API

- {{ link_ad_api('/api/vehicle/doors/layout') }}
- {{ link_ad_api('/api/vehicle/doors/status') }}
- {{ link_ad_api('/api/vehicle/doors/command') }}

## Description

This feature is available if the vehicle provides a software interface for the doors.
It can be used to create user interfaces for passengers or to control sequences at bus stops.

## Layout

Each door in a vehicle is assigned an array index. This assignment is vehicle dependent.
The layout API returns this information.
The description field is a string to display in the user interface, etc.
This is an arbitrary string and is not recommended to use for processing in applications.
Use the roles field to know doors for getting on and off.
Below is an example of the information returned by the layout API.

| Index | Description | Roles           |
| ----- | ----------- | --------------- |
| 0     | front right | -               |
| 1     | front left  | GET_ON          |
| 2     | rear right  | GET_OFF         |
| 3     | rear left   | GET_ON, GET_OFF |

## Status

The status API provides an array of door status. This array order is consistent with the layout API.

## Control

Use the command API to control doors.
Unlike the status and layout APIs, array index do not correspond to doors.
The command has a field to specify the target door index.
