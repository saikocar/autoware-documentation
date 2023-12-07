車両インターフェース設計
抽象的な
Vehicle Interface コンポーネントは、Autoware と車両の間のインターフェースを提供し、車両のドライブ バイ ワイヤ システムに制御信号を渡し、Autoware に返される車両情報を受信します。

1. 要件
目標:

Vehicle Interface コンポーネントは、Autoware コマンドを車両固有の形式に変換し、車両のステータスを車両固有の形式で Autoware メッセージに変換します。
Autoware と Vehicle コンポーネントの間のインターフェイスは抽象化されており、ハードウェアから独立しています。
このインターフェイスは拡張可能であるため、車両固有のコマンドを簡単に追加できます。例えばヘッドライトの制御。
目標以外:

車両からの応答の精度は定義されませんが、参考設計からの精度要件の例が例として提供されます。
応答速度は定義されません。
2. アーキテクチャ
Vehicle Interface コンポーネントは次のコンポーネントで構成されます。

速度/加速制御がドライブ・バイ・ワイヤ・システムによってサポートされている場合、制御コンポーネントからの車両コマンドを通過する生の車両コマンド・コンバーター・コンポーネント。それ以外の場合、制御コマンドは制御方法に従って変更されます (例: 加速マップを使用して、制御コンポーネントからの目標加速度を車両固有のアクセル/ブレーキ ペダル値に変換する)
Autoware と車両の間のインターフェースとして機能し、制御信号を通信し、車両に関する情報 (ステアリング出力、タイヤ角度など) を取得する車両インターフェース コンポーネント (車両固有)
各コンポーネントには Autoware の静的ノードが含まれていますが、各モジュールは動的にロードおよびアンロードできます (C++ クラスに対応)。Vehicle Interface コンポーネントのメカニズムは、次の図で示されています。

車両インターフェースの概要

車両インターフェースの概要

3. 特徴
Vehicle Interface コンポーネントは、次の機能と機能を提供できます。

基本機能

Autoware 制御コマンドを車両固有のコマンドに変換する
車両固有のステータス情報 (速度、ステアリング) を Autoware ステータス メッセージに変換する
診断

利用可能な機能をリストする
コントロールコンポーネントが車両インターフェースコンポーネントで利用できない機能を使用しようとした場合に警告を提供します。
車両のハードウェアに応じて、追加の機能や機能が追加される場合があります。いくつかの機能の例を以下に示します。

安全機能
手動介入により自動運転を解除します。
これは、緊急解除ボタンを使用するか、安全ドライバーが手動でステアリングホイールを回すかブレーキを押すことによって行うことができます。
オプションのコントロール
方向指示器
ハンドブレーキ
ヘッドライト
ハザードライト
ドア
ホーン
ワイパー
4. インターフェースとデータ構造
同じプロセス空間内で実行されている他のコンポーネントが車両インターフェース コンポーネントの機能と能力にアクセスするための車両インターフェース コンポーネントのインターフェースは、次のように定義されます。

フロムコントロール

作動コマンド
目標の加速、制動、ステアリング角度
企画から

車両固有のコマンド (オプションで、タイプごとに個別のメッセージ)
シフト
ドア
ワイパー
等
車両から

車両ステータスメッセージ
Autoware 固有の形式のメッセージに変換するための車両固有の形式のメッセージ
速度ステータス
ステアリングステータス (オプション)
シフトステータス（オプション）
方向指示器ステータス (オプション)
作動状態（オプション）
Vehicle Interface コンポーネントの出力インターフェイス:

車両への車両制御メッセージ
車両を駆動するための制御信号
車両のタイプ/プロトコルによって異なりますが、少なくともステアリングと速度のコマンドを含める必要があります
Autoware への車両ステータス メッセージ
作動ステータス
アクセル、ブレーキ、ステアリングの状態
車両オドメトリ (ローカリゼーションへの出力)
車両ねじれ情報
制御モード
車両が自律制御されているか手動制御されているかに関する情報
シフトステータス（オプション）
車両のシフト状態
方向指示器ステータス (オプション)
車両の方向指示器の状態
Vehicle Interface コンポーネントで使用されるオブジェクトと軌道のセマンティクスの内部表現のデータ構造は、次のように定義されます。

5. 懸念事項、仮定、制限事項
懸念事項

アーキテクチャのトレードオフとスケーラビリティ
仮定

制限事項

6. ODDによる精度要求の例
# Vehicle Interface design

## Abstract

The Vehicle Interface component provides an interface between Autoware and a vehicle that passes control signals to the vehicle’s drive-by-wire system and receives vehicle information that is passed back to Autoware.

## 1. Requirements

Goals:

- The Vehicle Interface component converts Autoware commands to a vehicle-specific format and converts vehicle status in a vehicle-specific format to Autoware messages.
- The interface between Autoware and the Vehicle component is abstracted and independent of hardware.
- The interface is extensible such that additional vehicle-specific commands can be easily added. For example, headlight control.

Non-goals:

- Accuracy of responses from the vehicle will not be defined, but example accuracy requirements from reference designs are provided as examples.
- Response speed will not be defined.

## 2. Architecture

The Vehicle Interface component consists of the following components:

- A Raw Vehicle Command Converter component that will pass through vehicle commands from the Control component if velocity/acceleration control is supported by the drive-by-wire system. Otherwise, the Control commands will be modified according to the control method (eg: converting a target acceleration from the Control component to a vehicle specific accel/brake pedal value through the use of an acceleration map)
- A Vehicle Interface component (vehicle specific) that acts as an interface between Autoware and a vehicle to communicate control signals and to obtain information about the vehicle (steer output, tyre angle etc)

Each component contains static nodes of Autoware, while each module can be dynamically loaded and unloaded (corresponding to C++ classes). The mechanism of the Vehicle Interface component is depicted by the following figures:

![Vehicle Interface overview](./image/vehicle_interface_architecture.png)

![Vehicle Interface overview](./image/vehicle_interface_overview.png)

## 3. Features

The Vehicle Interface component can provide the following features in functionality and capability:

- Basic functions

  - Converting Autoware control commands to vehicle specific command
  - Converting vehicle specific status information (velocity, steering) to Autoware status message

- Diagnostics
  - List available features
  - Provide a warning if the Control component tries to use a feature that is not available in the Vehicle Interface component

Additional functionality and capability features may be added, depending on the vehicle hardware. Some example features are listed below:

- Safety features
  - Disengage autonomous driving via manual intervention.
    - This can be done through the use of an emergency disengage button, or by a safety driver manually turning the steering wheel or pressing the brake
- Optional controls
  - Turn indicator
  - Handbrake
  - Headlights
  - Hazard lights
  - Doors
  - Horn
  - Wipers

## 4. Interface and Data Structure

The interface of the Vehicle Interface component for other components running in the same process space to access the functionality and capability of the Vehicle Interface component is defined as follows.

From Control

- Actuation Command
  - target acceleration, braking, and steering angle

From Planning

- Vehicle Specific Commands (optional and a separate message for each type)
  - Shift
  - Door
  - Wiper
  - etc

From the vehicle

- Vehicle status messages
  - Vehicle-specific format messages for conversion into Autoware-specific format messages
    - Velocity status
    - Steering status (optional)
    - Shift status (optional)
    - Turn signal status (optional)
    - Actuation status (optional)

The output interface of the Vehicle Interface component:

- Vehicle control messages to the vehicle
  - Control signals to drive the vehicle
  - Depends on the vehicle type/protocol, but should include steering and velocity commands at a minimum
- Vehicle status messages to Autoware
- Actuation Status
  - Acceleration, brake, steering status
- Vehicle odometry (output to Localization)
  - Vehicle twist information
- Control mode
  - Information about whether the vehicle is under autonomous control or manual control
- Shift status (optional)
  - Vehicle shift status
- Turn signal status (optional)
  - Vehicle turn signal status

The data structure for the internal representation of semantics for the objects and trajectories used in the Vehicle Interface component is defined as follows:

## 5. Concerns, Assumptions, and Limitations

Concerns

- Architectural trade-offs and scalability

Assumptions

-

Limitations

## 6. Examples of accuracy requirements by ODD
