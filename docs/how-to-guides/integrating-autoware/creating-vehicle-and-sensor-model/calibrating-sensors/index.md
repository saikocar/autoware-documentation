センサーを調整する
概要
Autoware は、知覚、位置特定、計画スタックへの入力として複数のセンサーが車両に取り付けられることを期待しています。Autoware は、フュージョン技術を使用して、複数のセンサーからの情報を結合します。これを効果的に機能させるには、すべてのセンサーを適切にキャリブレーションして座標系を揃える必要があり、センサーの位置を urdf ファイル ( sample_sensor_kitなど) または tf 起動ファイルのいずれかを使用して定義する必要があります。このドキュメントでは、キャリブレーション プロセス用のTIER IV のCalibrationToolsリポジトリについて説明します。このツールのインストールと使用方法については、TIER IV の CalibrationTools のページを参照してください。

他のキャリブレーション パッケージとメソッドを確認したい場合は、次のパッケージをチェックアウトできます。

チェックアウトできるその他のパッケージ
カメラのキャリブレーション
固有の校正
Navigation2 は、カメラの内部キャリブレーションに関する優れたチュートリアルを提供します。
AutoCore は軽量ツールを提供します。
ライダー間のキャリブレーション
Autocore の Lidar-Lidar キャリブレーション ツール
AutoCoreによって提供されるGitHub の LL-Calib は、オンライン/オフラインの 3D LiDAR 間キャリブレーション用の軽量ツールキットです。これは、ローカル マッピングと「GICP」手法に基づいて、メイン LIDAR とサブ LIDAR 間の関係を導き出します。ツールの使用方法、トラブルシューティングのヒント、および rosbag の例に関する情報は、上記のリンクで参照できます。

LiDAR カメラのキャリブレーション
MathWorks によって開発された Lidar Camera Calibrator アプリを使用すると、LIDAR センサーとカメラ間の剛体変換を対話的に推定できます。

https://ww2.mathworks.cn/help/lidar/ug/get-started-lidar-camera-calibrator.html

SensorsCalibration ツールボックス v0.1: Lidar カメラ キャリブレーション用のもう 1 つのオープン ソース メソッド。これは、自動キャリブレーションと手動キャリブレーションを含む、LiDAR からカメラへのキャリブレーションのプロジェクトです。

https://github.com/PJLab-ADG/SensorsCalibration/blob/master/lidar2camera/README.md

AutoCoreによって開発された、Lidar カメラ キャリブレーション用の使いやすい軽量ツールキットが提案されています。わずか 3 つのステップで、完全自動キャリブレーションが実行されます。

https://github.com/autocore-ai/calibration_tools/tree/main/lidar-cam-calib-関連

Lidar-IMU キャリブレーション
中国の浙江大学のAPRIL Labによって開発されたLI-Calib キャリブレーション ツールは、連続時間バッチ最適化に基づいて、6DoF 剛体変換と 3D LiDAR と IMU の間の時間オフセットをキャリブレーションするためのツールキットです。IMU ベースのコストと LiDAR のポイントからサーフェル (サーフェル = 表面要素) までの距離が同時に最小化されるため、一般的なシナリオでキャリブレーションの問題が十分に制約されます。

AutoCore は、オリジナルの LI-Calib ツールをフォークし、より一般的な用途のために Lidar 入力を上書きしました。このツールの使用方法、トラブルシューティングのヒント、および rosbag の例に関する情報は、GitHub の LI-Calib フォークで見つけることができます。
# Calibrating your sensors

## Overview

Autoware expects to have multiple sensors attached to the vehicle as input to perception, localization, and planning stack.
Autoware uses fusion techniques to combine information from multiple sensors.
For this to work effectively,
all sensors must be calibrated properly to align their coordinate systems, and their positions must be defined using either urdf files
(as in [sample_sensor_kit](https://github.com/autowarefoundation/sample_sensor_kit_launch/tree/main/sample_sensor_kit_description))
or as tf launch files.
In this documentation,
we will explain TIER IV's [CalibrationTools](https://github.com/tier4/CalibrationTools) repository for the calibration process.
Please look
at [Starting with TIER IV's CalibrationTools page](./calibration-tools) for installation and usage of this tool.

If you want to look at other calibration packages and methods, you can check out the following packages.

## Other packages you can check out

### Camera calibration

#### Intrinsic Calibration

- Navigation2 provides a [good tutorial for camera internal calibration](https://navigation.ros.org/tutorials/docs/camera_calibration.html).
- [AutoCore](https://autocore.ai/) provides a [light-weight tool](https://github.com/autocore-ai/calibration_tools/tree/main/camera_intrinsic_calib).

### Lidar-lidar calibration

#### Lidar-Lidar Calibration tool from Autocore

[LL-Calib on GitHub](https://github.com/autocore-ai/calibration_tools/tree/main/lidar-lidar-calib), provided by [AutoCore](https://autocore.ai/), is a lightweight toolkit for online/offline 3D LiDAR to LiDAR calibration. It's based on local mapping and "GICP" method to derive the relation between main and sub lidar. Information on how to use the tool, troubleshooting tips and example rosbags can be found at the above link.

### Lidar-camera calibration

Developed by MathWorks, The Lidar Camera Calibrator app enables you to interactively estimate the rigid transformation between a lidar sensor and a camera.

<https://ww2.mathworks.cn/help/lidar/ug/get-started-lidar-camera-calibrator.html>

SensorsCalibration toolbox v0.1: One more open source method for Lidar-camera calibration.
This is a project for LiDAR to camera calibration,including automatic calibration and manual calibration

<https://github.com/PJLab-ADG/SensorsCalibration/blob/master/lidar2camera/README.md>

Developed by [AutoCore](https://autocore.ai/), an easy-to-use lightweight toolkit for Lidar-camera-calibration is proposed. Only in three steps, a fully automatic calibration will be done.

<https://github.com/autocore-ai/calibration_tools/tree/main/lidar-cam-calib-related>

### Lidar-IMU calibration

Developed by [APRIL Lab](https://github.com/APRIL-ZJU) at Zhejiang University in China, the LI-Calib calibration tool is a toolkit for calibrating the 6DoF rigid transformation and the time offset between a 3D LiDAR and an IMU, based on continuous-time batch optimization.
IMU-based cost and LiDAR point-to-surfel (surfel = surface element) distance are minimized jointly, which renders the calibration problem well-constrained in general scenarios.

[AutoCore](https://autocore.ai/) has forked the original LI-Calib tool and overwritten the Lidar input for more general usage. Information on how to use the tool, troubleshooting tips and example rosbags can be found at the [LI-Calib fork on GitHub](https://github.com/autocore-ai/calibration_tools/tree/main/li_calib).
