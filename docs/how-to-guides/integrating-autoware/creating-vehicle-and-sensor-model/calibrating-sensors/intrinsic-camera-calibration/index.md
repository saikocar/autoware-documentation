固有のカメラキャリブレーション
概要
固有のカメラ キャリブレーションは、3D 情報を画像に投影するときに使用されるカメラの内部パラメータを決定するプロセスです。これらのパラメータには、焦点距離、光学中心、レンズ歪み係数が含まれます。カメラの固有キャリブレーションを実行するには、TIER IV の固有カメラ キャリブレーターツールを使用します。まず第一に、ドット、チェス、またはエイプリルタグ グリッド ボードなどのキャリブレーション ボードが必要です。このチュートリアルでは、7 cm の正方形で構成されるこの 7x7 チェス盤を使用します。

![chess_board](images/chess-board-7x7.png){ width="500"} このチュートリアル セクション用の 7x7 キャリブレーション チェス ボード。
Intrinsic Camera Calibratorページのキャリブレーション ボードのサンプルをいくつか示します。

チェス盤 ( 6x8 の例)
サークルドットボード（6x8の例）
Apriltag グリッドボード ( 3x4 の例)
キャリブレーション プロセスにバッグ ファイルを使用する場合、バッグ ファイルにはimage_rawカメラ センサーのトピックが含まれている必要がありますが、リアルタイムでキャリブレーションを実行できます。（推奨）

??? 注「固有のカメラ キャリブレーション プロセスの ROS 2 Bag の例」

```sh

Files:             rosbag2_2023_09_18-16_19_08_0.db3
Bag size:          12.0 GiB
Storage id:        sqlite3
Duration:          135.968s
Start:             Sep 18 2023 16:19:08.966 (1695043148.966)
End:               Sep 18 2023 16:21:24.934 (1695043284.934)
Messages:          4122
Topic information: Topic: /sensing/camera/camera0/image_raw | Type: sensor_msgs/msg/Image | Count: 2061 | Serialization Format: cdr

```
固有のカメラキャリブレーション
チュートリアルの他の調整パッケージとは異なり、このパッケージは初期化ファイルを作成する必要がありません。したがって、組み込みキャリブレーター パッケージの起動から始めることができます。

cd <YOUR-OWN-AUTOWARE-DIRECTORY>
source install/setup.bash
その後、固有キャリブレーターを起動します。

ros2 launch intrinsic_camera_calibrator calibrator.launch.xml
次に、初期設定とカメラ固有のキャリブレーション パネルが表示されます。キャリブレーション用の初期構成を設定します。

![chess_board](images/intrinsic-initial-configuration.png){ width="200"} 初期設定パネル
ソース オプション セクションから画像ソース (「ROS トピック」、「ROS バッグ」、または「画像ファイル」にすることができます) を設定します。「ROS トピック」ソースを使用してカメラを調整します。このパネルから画像ソースを選択した後、「ボード オプション」も設定する必要があります。キャリブレーションボードは次のとおりですChess board, Dot board or Apriltag。また、ボードパラメータを選択する必要があります。これを行うには、「ボードパラメータ」ボタンをクリックしてrow, column, and cellサイズを設定します。

画像ソースとボードパラメータを設定したら、キャリブレーションプロセスの準備が整います。スタートボタンをクリックするとTopic configurationパネルが表示されます。キャリブレーション プロセスに適切な Camera Raw トピックを選択してください。

![chess_board](images/intrinsic-topic-information.png){ width="200"} トピック構成パネル
これで校正の準備が整いました。XY軸、サイズ、スキューが異なるデータを収集してください。「データ収集統計の表示」をクリックすると、収集されたデータ統計を確認できます。詳細については、「内蔵カメラ キャリブレータ」のページを参照してください。

![chess_board](images/intrinsic-data-collecting.png){ align=center} 組み込みキャリブレーター インターフェイス
データ収集が完了したら、「調整」ボタンをクリックして調整プロセスを実行できます。キャリブレーションが完了すると、キャリブレーション結果の統計のデータが視覚化されます。「画像ビュータイプ「ソース未修正」を「ソース修正済み」に変更すると、キャリブレーション結果を確認できます。キャリブレーションが成功した場合（修正された画像に歪みがないはずです）、「保存」ボタンでキャリブレーション結果を保存できます。出力には という名前が付けられます<YOUR-CAMERA-NAME>_info.yaml。そのため、このファイルをカメラ ドライバーで直接使用します。

これは、tutorial_vehicle の固有のカメラ キャリブレーション プロセスをデモンストレーションするビデオです。 タイプ:ビデオ
# Intrinsic camera calibration

## Overview

Intrinsic camera calibration is the process
of determining the internal parameters of a camera
which will be used when projecting 3D information into images.
These parameters include focal length, optical center, and lens distortion coefficients.
In order to perform camera Intrinsic calibration,
we will use TIER IV's [Intrinsic Camera Calibrator](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/how_to_intrinsic_camera.md) tool.
First of all, we need a calibration board which can be dot, chess or apriltag grid board.
In this tutorial, we will use this 7x7 chess board consisting of 7 cm squares:

<figure markdown>
  ![chess_board](images/chess-board-7x7.png){ width="500"}
  <figcaption>
    Our 7x7 calibration chess board for this tutorial section.
  </figcaption>
</figure>

Here are some calibration board samples from [Intrinsic Camera Calibrator](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/how_to_intrinsic_camera.md) page:

- Chess boards ([6x8 example](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/resource/checkerboard_8x6.pdf))
- Circle dot boards ([6x8 example](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/resource/circle_8x6.pdf))
- Apriltag grid board ([3x4 example](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/resource/apriltag_grid_3x4.pdf))

If you want to use bag file for a calibration process,
the bag file must include `image_raw` topic of your camera sensor,
but you can perform calibration with real time.
(recommended)

??? note "ROS 2 Bag example for intrinsic camera calibration process"

    ```sh

    Files:             rosbag2_2023_09_18-16_19_08_0.db3
    Bag size:          12.0 GiB
    Storage id:        sqlite3
    Duration:          135.968s
    Start:             Sep 18 2023 16:19:08.966 (1695043148.966)
    End:               Sep 18 2023 16:21:24.934 (1695043284.934)
    Messages:          4122
    Topic information: Topic: /sensing/camera/camera0/image_raw | Type: sensor_msgs/msg/Image | Count: 2061 | Serialization Format: cdr

    ```

## Intrinsic camera calibration

Unlike other calibration packages in our tutorials,
this package does not need to create an initialization file.
So we can start with launching intrinsic calibrator package.

```bash
cd <YOUR-OWN-AUTOWARE-DIRECTORY>
source install/setup.bash
```

After that, we will launch intrinsic calibrator:

```bash
ros2 launch intrinsic_camera_calibrator calibrator.launch.xml
```

Then, initial configuration and camera intrinsic calibration panels will show up.
We set initial configurations for our calibration.

<figure markdown>
  ![chess_board](images/intrinsic-initial-configuration.png){ width="200"}
  <figcaption>
    Initial configuration panel
  </figcaption>
</figure>

We set our image source (it can be "ROS topic", "ROS bag" or "Image files")
from the source options section.
We will calibrate our camera with "ROS topic" source.
After selecting an image source from this panel, we need to configure "Board options" as well.
The calibration board can be `Chess board, Dot board or Apriltag`.
Also, we need to select board parameters,
to do that, click the "Board parameters" button and set `row, column, and cell` size.

After the setting of image source and board parameters, we are ready for the calibration process.
Please click the start button, you will see `Topic configuration` panel.
Please select the appropriate camera raw topic for the calibration process.

<figure markdown>
  ![chess_board](images/intrinsic-topic-information.png){ width="200"}
  <figcaption>
    Topic configuration panel
  </figcaption>
</figure>

Then you are ready to calibration.
Please collect data with different X-Y axis, sizes and skews.
You can see your collected data statistics with the clicking view data collection statistics.
For more information,
please refer to [Intrinsic Camera Calibrator](https://github.com/tier4/CalibrationTools/blob/tier4/universe/sensor/docs/how_to_intrinsic_camera.md) page.

<figure markdown>
  ![chess_board](images/intrinsic-data-collecting.png){ align=center}
  <figcaption>
    Intrinsic calibrator interface
  </figcaption>
</figure>

After the data collection is completed,
you can click the "Calibrate" button to perform the calibration process.
After the calibration is completed,
you will see data visualization for the calibration result statistics.
You can observe your calibration results with changing "Image view type "Source unrectified"
to "Source rectified".
If your calibration is successful (there should be no distortion in the rectified image),
you can save your calibration results with "Save" button.
The output will be named as `<YOUR-CAMERA-NAME>_info.yaml`.
So, you use this file with your camera driver directly.

Here is the video
for demonstrating the intrinsic camera calibration process on tutorial_vehicle:
![type:video](https://youtube.com/embed/jN77AdGFrGU)
