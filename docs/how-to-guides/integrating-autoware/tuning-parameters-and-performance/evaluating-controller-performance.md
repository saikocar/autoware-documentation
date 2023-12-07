コントローラーの性能を評価する
control_performance_analysisこのページでは、パッケージを使用してコントローラーを評価する方法を説明します。

control_performance_analysis制御モジュールの追従性能を解析し、車両の走行状態を監視するパッケージです。

パッケージの詳細情報が必要な場合は、control_performance_analysisを参照してください。

使い方
運転する前に
1. まず、Autoware を起動する必要があります。このツールは実際の車両の運転でも使用できます
2.車両を初期化し、ゴール位置を送信してルートを作成する
Autoware の起動に問題がある場合は、チュートリアルページを参照してください。
3. control_パフォーマンス_分析パッケージを起動します
ros2 launch control_performance_analysis controller_performance_analysis.launch.xml
このコマンドを実行すると、トピックに駆動モニターとエラー変数が表示されるようになります。
4. ソースターミナルで PlotJuggler を実行します
source ~/autoware/install/setup.bash
ros2 run plotjuggler plotjuggler
お使いのコンピュータに PlotJuggler がインストールされていない場合は、こちらのインストール ガイドラインを参照してください。
5. バッファ サイズを増やし (最大は 100)、レイアウトを次からインポートします。/autoware.universe/control/control_performance_analysis/config/controller_monitor.xml
レイアウトをインポートした後、以下にリストされているトピックを指定してください。
/localization/kinematic_state
/車両/ステータス/ステアリング_ステータス
/コントロールパフォーマンス/運転ステータス
/control_パフォーマンス/パフォーマンス_vars
ボックスにチェックを入れてIf present, use the timestamp in the field [header.stamp]、[OK] を選択してください。
PlotJuggler を開始する

6. これで、運転を開始できます。PlotJuggler ですべてのパフォーマンス変数と駆動変数が表示されるはずです。
コントローラーモニター

運転後
1. 統計出力とすべてのデータをエクスポートして比較したり、後で使用したりできます
統計データを使用すると、(最小、最大、平均) などのすべての統計値をエクスポートして、コントローラーを比較できます。
CVS エクスポート

すべてのデータをエクスポートして後で使用することもできます。再度調査するには、PlotJuggler を起動した後、.cvsデータ セクションからファイルをインポートします。
データのインポート

チップ
車両位置をプロットできます。2 つの曲線を選択し (CTRL キーを押したまま)、マウスの右ボタンを使用してドラッグ アンド ドロップします。Help -> Cheatsheet詳細なヒントについては、PlotJuggler の にアクセスしてください。
XY 曲線のプロット

プロット内にノイズのある曲線が多すぎる場合は、ここからodom_intervalと を調整して、ノイズのあるデータを避けることができます。low_pass_filter_gain
# Evaluating the controller performance

This page shows how to use `control_performance_analysis` package to evaluate the controllers.

`control_performance_analysis` is the package to analyze the tracking performance of a control module
and monitor the driving status of the vehicle.

If you need more detailed information about package, refer to the [control_performance_analysis](https://github.com/autowarefoundation/autoware.universe/tree/main/control/control_performance_analysis).

## How to use

### Before Driving

#### 1. Firstly you need to launch Autoware. You can also use this tool with real vehicle driving

#### 2. Initialize the vehicle and send goal position to create route

- If you have any problem with launching Autoware, please see the [tutorials](../../../tutorials/index.md) page.

#### 3. Launch the control_performance_analysis package

```bash
ros2 launch control_performance_analysis controller_performance_analysis.launch.xml
```

- After this command, you should be able to see the driving monitor and error variables in topics.

#### 4. Run the PlotJuggler in sourced terminal

```bash
source ~/autoware/install/setup.bash
```

```bash
ros2 run plotjuggler plotjuggler
```

- If you do not have PlotJuggler in your computer, please refer [here](https://github.com/facontidavide/PlotJuggler#installation) for installation guideline.

#### 5. Increase the buffer size (maximum is 100), and import the layout from `/autoware.universe/control/control_performance_analysis/config/controller_monitor.xml`

- After import the layout, please specify the topics that are listed below.

> - /localization/kinematic_state
> - /vehicle/status/steering_status
> - /control_performance/driving_status
> - /control_performance/performance_vars

- Please mark the `If present, use the timestamp in the field [header.stamp]` box, then select the OK.

![Start PlotJuggler](images/evaluating-controller-performance/start-plotjuggler.png)

#### 6. Now, you can start to driving. You should see all the performance and driving variables in PlotJuggler

![Controller Monitor](images/evaluating-controller-performance/controller-monitor.png)

### After Driving

#### 1. You can export the statistical output and all data to compare and later usage

- With statistical data, you can export the all statistical values like (min, max, average) to compare the controllers.

![CVS Export](images/evaluating-controller-performance/export-cvs.png)

- You can also export all data to later use. To investigate them again, after launch PlotJuggler, import the `.cvs` file from data section.

![Import Data](images/evaluating-controller-performance/import-data.png)

### Tips

- You can plot the vehicle position. Select the two curve (keeping CTRL key pressed) and Drag & Drop them using the **RIGHT Mouse button**. Please visit the `Help -> Cheatsheet` in PlotJuggler to see more tips about it.

![Plot XY Curve](images/evaluating-controller-performance/plot-xy.png)

- If you see too much noised curve in plots, you can adjust the `odom_interval` and `low_pass_filter_gain` from [here](https://github.com/autowarefoundation/autoware.universe/blob/main/control/control_performance_analysis/config/control_performance_analysis.param.yaml) to avoid noised data.
