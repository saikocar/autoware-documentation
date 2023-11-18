# ランダムテストシミュレーション

!!! 注記

    シナリオシミュレーターを実行するには、Autowareのビルドとインストールに加えていくつかの追加手順が必要となるため、先に進む前にまず[シナリオシミュレーターのインストール](installation.md)が完了していることを確認してください。

## 実行手順

1. Autowareとシナリオシミュレーターがビルドされたワークスペースディレクトリに移動します。

2. ワークスペースセットアップスクリプトを実行します:

   ```bash
   source install/setup.bash
   ```

3. シミュレーションを実行します:

   ```bash
   ros2 launch random_test_runner random_test.launch.py \
     architecture_type:=awf/universe \
     sensor_model:=sample_sensor_kit \
     vehicle_model:=sample_vehicle
   ```

![random_test_runner](images/random_test_runner.png)

サポートされているパラメータの詳細については[random_test_runnerのドキュメント](https://github.com/tier4/scenario_simulator_v2/blob/master/docs/user_guide/random_test_runner/README.md#node-parameters)を参照してください。
