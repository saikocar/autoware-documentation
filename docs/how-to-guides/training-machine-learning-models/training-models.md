モデルのトレーニングとデプロイ
概要
Autoware は、2D および 3D オブジェクトの検出、信号認識などを含む幅広いタスクに合わせて調整された、包括的な機械学習モデルを提供します。これらのモデルは、open-mmlabの広範なリポジトリを利用して細心の注意を払ってトレーニングされています。提供されたスクリプトを活用し、トレーニング手順に従うことで、独自のデータセットを使用してこれらのモデルをトレーニングし、特定のニーズに合わせて調整することができます。

さらに、 mmdeployリポジトリを使用して、トレーニング済みモデルを Autoware にデプロイするための重要な変換スクリプトが見つかります。

信号機分類子モデルのトレーニング
Autoware 内の信号機分類モデルは、mmlab/pretrainedリポジトリを使用してトレーニングされています。Autoware は、EfficientNet-b1 および MobileNet-v2 アーキテクチャに基づいた事前トレーニング済みモデルを提供します。これらのモデルを微調整するために、トレーニング用に 58,600 枚、評価用に 14,800 枚、テスト用に 10,000 枚の合計 83,400 枚の画像が使用されました。これらの画像は日本の信号機を表しており、TIER IV の内部データセットを使用してトレーニングされました。

名前	入力サイズ	テストの精度
EfficientNet-b1	128×128	99.76%
モバイルネットv2	224×224	99.81%
信号機分類器モデルの包括的なトレーニング手順の詳細は、「traffic_light_classifier」パッケージに付属の Readme ファイルに記載されています。これらの手順では、独自のデータセットを使用してモデルをトレーニングするプロセスをガイドします。トレーニングを容易にするために、トレーニング プロセス中に活用できる 3 つの異なるクラス (緑、黄、赤) を含むサンプル データセットも提供しました。

信号機分類モデルをトレーニングするための詳細な手順については、ここを参照してください。
# Training and Deploying Models

## Overview

The Autoware offers a comprehensive array of machine learning models, tailored for a wide range of tasks including 2D and 3D object detection,
traffic light recognition and more. These models have been meticulously trained utilizing **[open-mmlab](https://github.com/open-mmlab)**'s extensive repositories.
By leveraging the provided scripts and following the training steps, you have the capability to train these models using your own dataset,
tailoring them to your specific needs.

Furthermore, you will find the essential conversion scripts to deploy your trained models into Autoware using the **[mmdeploy](https://github.com/open-mmlab/mmdeploy)** repository.

## Training traffic light classifier model

The traffic light classifier model within the Autoware has been trained using the **[mmlab/pretrained](https://github.com/open-mmlab/mmpretrain)** repository.
The Autoware offers pretrained models based on EfficientNet-b1 and MobileNet-v2 architectures.
To fine-tune these models, a total of 83,400 images were employed, comprising 58,600 for training,
14,800 for evaluation, and 10,000 for testing. These images represent Japanese traffic lights and were trained using TIER IV's internal dataset.

| Name            | Input Size | Test Accuracy |
| --------------- | ---------- | ------------- |
| EfficientNet-b1 | 128 x 128  | 99.76%        |
| MobileNet-v2    | 224 x 224  | 99.81%        |

Comprehensive training instructions for the traffic light classifier model are detailed within
the readme file accompanying **"traffic_light_classifier"** package. These instructions will guide you through
the process of training the model using your own dataset. To facilitate your training, we have also provided
an example dataset containing three distinct classes (green, yellow, red), which you can leverage during the training process.

Detailed instructions for training the traffic light classifier model can be found **[here](https://github.com/autowarefoundation/autoware.universe/blob/main/perception/traffic_light_classifier/README.md)**.

<!--

Training traffic light detection model and lidar CenterPoint model will be added there.

-->
