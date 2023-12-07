車両監視
AD API は、遠隔監視や乗員の可視化などのために、車両の現在の状態を提供します。監視したいデータに応じて、以下の API を使用してください。

車両状態
車両のステータスは、運動学、インジケーター、寸法などの基本情報を提供します。これにより、遠隔操作者は車両の位置と速度を知ることができます。FMS などのアプリケーションの場合、スタックした車両や急ブレーキがかかった車両など、支援が必要な車両を見つけるのに役立ちます。車両の寸法から物体までの実際の距離を求めることも可能です。

計画要素
計画要素は、車両の計画ステータスを提供します。HMIはこれを利用して車両の急な動きを警告したり、停止理由を同乗者と共有したりすることで快適な運転を実現します。

検出されたオブジェクト
知覚は、 Autoware によって検出されたオブジェクトを提供します。HMI はこれを使用して、車両の周囲のオブジェクトを視覚化できます。
# Vehicle monitoring

AD API provides current vehicle status for remote monitoring, visualization for passengers, etc.
Use the API below depending on the data you want to monitor.

## Vehicle status

The [vehicle status](../features/vehicle-status.md) provides basic information such as kinematics, indicators, and dimensions.
This allows a remote operator to know the position and velocity of the vehicle.
For applications such as FMS, it can help find vehicles that need assistance, such as vehicles that are stuck or brake suddenly.
It is also possible to determine the actual distance to an object from the vehicle dimensions.

## Planning factors

The [planning factors](../features/planning-factors.md) provides the planning status of the vehicle.
HMI can use this to warn of sudden movements of the vehicle, and to share the stop reason with passengers for comfortable driving.

## Detected objects

The [perception](../features/perception.md) provides the objects detected by Autoware.
HMI can use this to visualize objects around the vehicle.
