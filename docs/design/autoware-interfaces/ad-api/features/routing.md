ルーティング
関連するAPI
{{ link_ad_api('/api/routing/state') }}
{{ link_ad_api('/api/routing/route') }}
{{ link_ad_api('/api/routing/set_route_points') }}
{{ link_ad_api('/api/routing/set_route') }}
{{ link_ad_api('/api/routing/clear_route') }}
説明
この API は目的地と経由地を管理します。ウェイポイントはストップのようなものではなく、単なる通過点であることに注意してください。つまり、Autoware は複数のストップがあるルートをサポートしていないため、アプリケーションがそれを分割して切り替える必要があります。ルートを設定するには 2 つの方法があります。1 つはポーズを使用する一般的な方法で、もう 1 つはマップに依存する方法です。

州
ルートステート

州	説明
設定を解除する	ルートは設定されていません。ルートリクエストを待っています。
セット	ルートが設定されています。
到着した	車は目的地に到着しました。
変化	ルートを変更しようとしています。まだ実装されていません。
目標の修正
Autoware は、目標に到達できない場合 (たとえば、指定された目標に障害物がある場合)、代替の目標を探しようとします。API からルートを設定する場合、アプリケーションは、そのような状況で Autoware がゴールポーズを調整できるようにするかどうかを選択できます。false に設定すると、指定された目標に到達するまで Autoware が停止する可能性があります。

オプション	説明
allow_goal_modification	true の場合、目標の変更を許可します。

# Routing

## Related API

- {{ link_ad_api('/api/routing/state') }}
- {{ link_ad_api('/api/routing/route') }}
- {{ link_ad_api('/api/routing/set_route_points') }}
- {{ link_ad_api('/api/routing/set_route') }}
- {{ link_ad_api('/api/routing/clear_route') }}

## Description

This API manages destination and waypoints. Note that waypoints are not like stops and just points passing through.
In other words, Autoware does not support the route with multiple stops, the application needs to split it up and switch them.
There are two ways to set the route. The one is a generic method that uses pose, another is a map-dependent.

## States

![route-state](./routing/state.drawio.svg)

| State    | Description                                        |
| -------- | -------------------------------------------------- |
| UNSET    | The route is not set. Waiting for a route request. |
| SET      | The route is set.                                  |
| ARRIVED  | The vehicle has arrived at the destination.        |
| CHANGING | Trying to change the route. Not implemented yet.   |

## Goal modification

Autoware tries to look for an alternate goal when goal is unreachable (e.g., when there is an obstacle on the given goal). When setting a route from the API, applications can choose whether they allow Autoware to adjust goal pose in such situation. When set false, Autoware may get stuck until the given goal becomes reachable.

| Option                  | Description                       |
| ----------------------- | --------------------------------- |
| allow_goal_modification | If true, allow goal modification. |
