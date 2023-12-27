# モデルのトレーニングとデプロイ

## 概要

Autowareは、2Dおよび3Dオブジェクトの検出、信号認識などを含む幅広いタスクに合わせて調整された、包括的な機械学習モデルを提供します。
これらのモデルは、**[open-mmlab](https://github.com/open-mmlab)**'の広範なリポジトリを利用して細心の注意を払ってトレーニングされています。
提供されたスクリプトを活用し、トレーニング手順に従うことで、独自のデータセットを使用してこれらのモデルをトレーニングし、
特定のニーズに合わせて調整することができます。

さらに、**[mmdeploy](https://github.com/open-mmlab/mmdeploy)**リポジトリを使用して、トレーニング済みモデルをAutowareにデプロイするための重要な変換スクリプトが見つかります。

## 信号機分類子モデルのトレーニング

Autoware内の信号機分類モデルは、**[mmlab/pretrained](https://github.com/open-mmlab/mmpretrain)**リポジトリを使用してトレーニングされています。
Autowareは、EfficientNet-b1およびMobileNet-v2アーキテクチャに基づいた事前トレーニング済みモデルを提供します。
これらのモデルを微調整するために、トレーニング用に58,600 枚、評価用に 14,800 枚、テスト用に10,000枚の合計83,400枚の画像が使用されました。
これらの画像は日本の信号機を表しており、TIER IVの内部データセットを使用してトレーニングされました。

| 名前            | 入力サイズ | テスト精度 |
| --------------- | ---------- | ------------- |
| EfficientNet-b1 | 128 x 128  | 99.76%        |
| MobileNet-v2    | 224 x 224  | 99.81%        |

信号機分類器モデルの包括的なトレーニング手順の詳細は、
**"traffic_light_classifier"**パッケージに付属のReadmeファイルに記載されています。
これらの手順では、独自のデータセットを使用してモデルをトレーニングするプロセスをガイドします。
トレーニングを容易にするために、トレーニング プロセス中に活用できる 3 つの異なるクラス (緑、黄、赤) を含むサンプル データセットも提供しました。

信号機分類モデルをトレーニングするための詳細な手順については**[こちら](https://github.com/autowarefoundation/autoware.universe/blob/main/perception/traffic_light_classifier/README.md)**を参照してください。

<!--

Training traffic light detection model and lidar CenterPoint model will be added there.

-->
