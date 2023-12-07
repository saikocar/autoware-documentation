車両の運行
介入の要請
介入要求 (RTI) は、オペレーターが手動運転モードに切り替えることを要求する機能です。テイクオーバーリクエスト（TOR）とも呼ばれます。RTI 用のインターフェイスについては現在検討中です。ここでは、MRM 状態が NORMAL でない場合に手動運転が要求されたとします。詳細については、フェールセーフを参照してください。

協力のお願い
協力要請（RTC）は、自動運転モードにおいてオペレーターが判断をサポートする機能です。Autoware は通常、独自の決定に基づいて車両を運転しますが、複雑な状況ではオペレーターが独自の決定を行うことを好む場合があります。RTC は決定をオーバーライドするだけで動作モードを変更する必要がないため、RTC とは異なり、車両は自動運転を継続できます。詳細は連携をご覧ください。
# Vehicle operation

## Request to intervene

Request to intervene (RTI) is a feature that requires the operator to switch to manual driving mode. It is also called Take Over Request (TOR).
Interfaces for RTI are currently being discussed. For now assume that manual driving is requested if the MRM state is not NORMAL.
See [fail-safe](../features/fail-safe.md) for details.

## Request to cooperate

Request to cooperate (RTC) is a feature that the operator supports the decision in autonomous driving mode.
Autoware usually drives the vehicle using its own decisions, but the operator may prefer to make their own decisions in complex situations.
Since RTC only overrides the decision and does not need to change operation mode, the vehicle can continue autonomous driving, unlike RTC.
See [cooperation](../features/cooperation.md) for details.
