# センサーを調整する

## 概要

Autoware は、知覚、位置特定、計画スタックへの入力として複数のセンサーが車両に取り付けられることを期待しています。
Autoware は、フュージョン技術を使用して、複数のセンサーからの情報を結合します。
これを効果的に機能させるには、
すべてのセンサーを適切にキャリブレーションして座標系を揃える必要があり、センサーの位置を urdf ファイル
([sample_sensor_kit](https://github.com/autowarefoundation/sample_sensor_kit_launch/tree/main/sample_sensor_kit_description)など)
または tf 起動ファイルのいずれかを使用して定義する必要があります。
このドキュメントでは、
キャリブレーション プロセス用のTIER IV の[CalibrationTools](https://github.com/tier4/CalibrationTools)リポジトリについて説明します。
このツールのインストールと使用方法については、
[STIER IV の CalibrationTools のページ](./calibration-tools)を参照してください。

他のキャリブレーション パッケージとメソッドを確認したい場合は、次のパッケージをチェックアウトできます。

## チェックアウトできるその他のパッケージ

### カメラのキャリブレーション

#### 固有の校正

- Navigation2は、[カメラの内部キャリブレーションに関する優れたチュートリアル](https://navigation.ros.org/tutorials/docs/camera_calibration.html)を提供します。
- [AutoCore](https://autocore.ai/)は[軽量ツール](https://github.com/autocore-ai/calibration_tools/tree/main/camera_intrinsic_calib)を提供します。

### ライダー間のキャリブレーション

#### Autocore の Lidar-lidarキャリブレーション ツール

[AutoCore](https://autocore.ai/)によって提供される[GitHub の LL-Calib](https://github.com/autocore-ai/calibration_tools/tree/main/lidar-lidar-calib)は、オンライン/オフラインの 3D LiDAR 間キャリブレーション用の軽量ツールキットです。これは、ローカル マッピングと"GICP"手法に基づいて、メイン LIDAR とサブ LIDAR 間の関係を導き出します。ツールの使用方法、トラブルシューティングのヒント、および rosbag の例に関する情報は、上記のリンクで参照できます。

### Lidar-カメラキャリブレーション

MathWorks によって開発された Lidar Camera Calibrator アプリを使用すると、LIDAR センサーとカメラ間の剛体変換を対話的に推定できます。

<https://ww2.mathworks.cn/help/lidar/ug/get-started-lidar-camera-calibrator.html>

SensorsCalibration toolbox v0.1: Lidar カメラ キャリブレーション用のもう 1 つのオープン ソース メソッド。
これは、自動キャリブレーションと手動キャリブレーションを含む、LiDAR からカメラへのキャリブレーションのプロジェクトです

<https://github.com/PJLab-ADG/SensorsCalibration/blob/master/lidar2camera/README.md>

[AutoCore](https://autocore.ai/)によって開発された、Lidar カメラ キャリブレーション用の使いやすい軽量ツールキットが提案されています。わずか 3 つのステップで、完全自動キャリブレーションが実行されます。

<https://github.com/autocore-ai/calibration_tools/tree/main/lidar-cam-calib-related>

### Lidar-IMUキャリブレーション

中国の浙江大学の[APRIL Lab](https://github.com/APRIL-ZJU)によって開発されたLI-Calib キャリブレーション ツールは、連続時間バッチ最適化に基づいて、6DoF 剛体変換と 3D LiDAR と IMU の間の時間オフセットをキャリブレーションするためのツールキットです。
IMU ベースのコストと LiDAR のポイントからサーフェル (サーフェル = 表面要素) までの距離が同時に最小化されるため、一般的なシナリオでキャリブレーションの問題が十分に制約されます。

[AutoCore](https://autocore.ai/)は、オリジナルの LI-Calib ツールをフォークし、より一般的な用途のために Lidar 入力を上書きしました。このツールの使用方法、トラブルシューティングのヒント、および rosbag の例に関する情報は、[GitHub の LI-Calib フォーク](https://github.com/autocore-ai/calibration_tools/tree/main/li_calib)で見つけることができます。
