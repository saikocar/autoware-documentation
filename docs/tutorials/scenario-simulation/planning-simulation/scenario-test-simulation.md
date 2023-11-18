# シナリオテストシミュレーション

!!! 注記

    シナリオシミュレーターを実行するには、Autowareのビルドとインストールに加えていくつかの追加手順が必要となるため、先に進む前にまず[シナリオシミュレーターのインストール](installation.md)が完了していることを確認してください。

## 実行手順

1. Autowareとシナリオシミュレーターがビルドされたワークスペースディレクトリに移動します。ワークスペースセットアップスクリプトを実行します。
2. ワークスペースセットアップスクリプトを実行します:

   ```bash
   source install/setup.bash
   ```

3. シミュレーションを実行します:

   ```bash
   ros2 launch scenario_test_runner scenario_test_runner.launch.py \
     architecture_type:=awf/universe \
     record:=false \
     scenario:='$(find-pkg-share scenario_test_runner)/scenario/sample.yaml' \
     sensor_model:=sample_sensor_kit \
     vehicle_model:=sample_vehicle
   ```

![scenario_test_runner](images/scenario_test_runner.png)

[ビデオチュートリアルを参照する](https://user-images.githubusercontent.com/102840938/206996920-758b62ae-270a-497c-8a72-f9e4867df695.mp4)
