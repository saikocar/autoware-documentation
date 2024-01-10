# 車両をAWSIM環境に統合する方法

## 概要

[AWSIM](https://github.com/tier4/AWSIM)は、自動運転システムのトレーニングと評価のために
TIER IVによって設計されたオープンソースシミュレーターです。
現実世界のさまざまなシナリオをシミュレートするための現実的な仮想環境を提供し、
ユーザーが実際の車両に展開する前に自律システムをテストして改良できるようにします。

## Unityプロジェクトのセットアップ

環境と車両を AWSIM シミュレーションに追加するには、
コンピューター上に[Unity](https://unity.com/)環境をセットアップする必要があります。
[`Setup Unity Project`](https://tier4.github.io/AWSIM/GettingStarted/SetupUnityProject/)ドキュメントページの手順に従って、
コンピューターに
Unity 環境をセットアップしてください。

<figure markdown>
  ![setup-unity-project](images/awsim-unity-project.png){ align=center }
  <figcaption>
  AWSSIM Unity セットアップ
  </figcaption>
</figure>

## 新しい車両の統合

車両をAWSIM環境に組み込むには
車両の3Dモデルファイル(.dae、.fbx)が必要です。
独自の車両をAWSIMプロジェクト環境に追加するには、[`Add New Vehicle
documentation page`](https://tier4.github.io/AWSIM/Components/Vehicle/AddNewVehicle/AddAVehicle/)の手順を
参照してください。これらの手順では、車両の
センサー URDF 設計を構成します。
次の画像では、AWSIM環境でのチュートリアル車両を示しています。

<figure markdown>
  ![tutorial-vehicle-awsim-integration](images/tutorial-vehicle-awsim-integration.png){ align=center }
  <figcaption>
  AWSIM Unity 環境のチュートリアル用車両
  </figcaption>
</figure>

## 環境の統合

AWSIM用のカスタム3D環境を作成することは可能ですが、
.fbx ファイル形式に従うことをお勧めします。
Unity とシームレスに統合するには、
マテリアルとテクスチャを別のディレクトリに保存する必要があります。この形式により、
マテリアルのインポートとインポート中の置換が容易になります。
カスタム環境をAWSIM ロジェクト環境に追加するには、
[`Add Environment documentation page`](https://tier4.github.io/AWSIM/Components/Environment/AddNewEnvironment/AddEnvironment/)
の手順を参照してください。

<figure markdown>
  ![tutorial-vehicle-awsim-environment](images/tutorial-vehicle-environment.png){ align=center }
  <figcaption>
  チュートリアル車両 AWSIM Unity 環境
  </figcaption>
</figure>

## その他

さらに、[`AWSIM documentation`](https://tier4.github.io/AWSIM/)
に記載されている関連ドキュメントの手順に従って、
交通量と NPC を組み込み、lanlet2 マップを使用して点群マップを生成し、
その他のタスクを実行することができます。
