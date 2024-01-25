# コントローラーの性能を評価する

このページでは、`control_performance_analysis`パッケージを使用してコントローラーを評価する方法を説明します。

`control_performance_analysis`は制御モジュールの追従性能を解析し、車両の走行状態を監視するパッケージです。

パッケージの詳細情報が必要な場合は[control_performance_analysis](https://github.com/autowarefoundation/autoware.universe/tree/main/control/control_performance_analysis)を参照してください。

## 使い方

### 運転する前に

#### 1. Fまず、Autoware を起動する必要があります。このツールは実際の車両の運転でも使用できます

#### 2. 車両を初期化し、ゴール位置を送信してルートを作成する

- Autoware の起動に問題がある場合は、[チュートリアル](../../../tutorials/index.md)ページを参照してください。

#### 3. control_performance_analysisパッケージを起動します

```bash
ros2 launch control_performance_analysis controller_performance_analysis.launch.xml
```

- このコマンドを実行すると、トピックに駆動モニターとエラー変数が表示されるようになります。

#### 4. ソースしたターミナルで PlotJuggler を実行します

```bash
source ~/autoware/install/setup.bash
```

```bash
ros2 run plotjuggler plotjuggler
```

- お使いのコンピュータに PlotJuggler がインストールされていない場合は、[こちら](https://github.com/facontidavide/PlotJuggler#installation)のインストール ガイドラインを参照してください。

#### 5. バッファ サイズを増やし(最大100),レイアウトを`/autoware.universe/control/control_performance_analysis/config/controller_monitor.xml`からインポートします

- レイアウトをインポートした後、以下にリストされているトピックを指定してください。

> - /localization/kinematic_state
> - /vehicle/status/steering_status
> - /control_performance/driving_status
> - /control_performance/performance_vars

- `[header.stamp]フィールドにライムスタンプが存在し、使用する場合は`ボックスにチェックを入れて、OKを選択してください。

![Start PlotJuggler](images/evaluating-controller-performance/start-plotjuggler.png)

#### 6. これで、運転を開始できます。PlotJuggler ですべてのパフォーマンス変数と駆動変数が表示されるはずです。

![Controller Monitor](images/evaluating-controller-performance/controller-monitor.png)

### 運転後

#### 1. 統計出力とすべてのデータをエクスポートして比較したり、後で使用したりできます

- 統計データを使用すると、(最小、最大、平均) などのすべての統計値をエクスポートして、コントローラーを比較できます。

![CVS Export](images/evaluating-controller-performance/export-cvs.png)

- すべてのデータをエクスポートして後で使用することもできます。再度調査するには、PlotJuggler を起動した後、データ セクションから`.cvs`ファイルをインポートします。

![Import Data](images/evaluating-controller-performance/import-data.png)

### チップ

- 車両位置をプロットできます。2 つの曲線を選択し (CTRL キーを押したまま)、**マウスの右ボタン**を使用してドラッグ アンド ドロップします。 詳細なヒントについては、PlotJuggler の`Help -> Cheatsheet`にアクセスしてください

![Plot XY Curve](images/evaluating-controller-performance/plot-xy.png)

- プロット内にノイズのある曲線が多すぎる場合は、`odom_interval`と`low_pass_filter_gain`を[ここ](https://github.com/autowarefoundation/autoware.universe/blob/main/control/control_performance_analysis/config/control_performance_analysis.param.yaml)から調整して、ノイズのあるデータを避けることができます。
