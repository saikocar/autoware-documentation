Autoware リポジトリの作成
メタリポジトリとは何ですか?
メタリポジトリとは複数のリポジトリを管理するリポジトリのことで、Autowareもその 1 つです。これは、他のリポジトリの参照、構成、バージョン管理のための集中管理ポイントとして機能します。これを実現するために、Autoware メタ リポジトリには、autoware.repos複数のリポジトリを管理するためのファイルが含まれています。VCSツール (バージョン管理システム)を使用して.repos ファイルを処理します。VCS は、複数のリポジトリからインポート、エクスポート、プルする機能を提供します。VCS は、Autoware をワークスペースに構築するために必要なすべてのリポジトリをインポートするために使用されます。VCS および .repos ファイルの使用法についてはドキュメントを参照してください。

Autoware メタリポジトリを作成およびカスタマイズする方法
1. Autoware メタリポジトリを作成する
Autoware を車両に統合する場合、最初のステップは Autoware メタ リポジトリを作成することです。

簡単な方法の 1 つは、Autoware リポジトリをフォークしてクローンを作成することです。(リポジトリをフォークする方法については、GitHub Docsを参照してください)

このガイドでは、Autoware は に統合されますtutorial_vehicle (注: 複数のタイプの車両を設定する場合は、autoware.vehicle_Aまたは のような接尾辞を追加することautoware.vehicle_Bをお勧めします)。最初のステップとして、autowareリポジトリにアクセスし、フォーク ボタンをクリックしてください。フォークプロセスは次のようになります。
![forking-autoware_repository](images/forking-autoware_repository.png){ align=center }tutorial_vehicle のフォーク デモンストレーションのサンプル
次に、「フォークの作成」ボタンをクリックして続行します。その後、ローカル システム上にフォーク リポジトリのクローンを作成できます。

git clone https://github.com/YOUR_NAME/autoware.<YOUR-VEHICLE>.git
たとえば、これはドキュメント用です。

git clone https://github.com/leo-drive/autoware.tutorial_vehicle.git
1.1 車両個別リポジトリの作成
Autoware を個々の車両に統合するには、次のリポジトリもフォークして変更する必要があります。

Sample_sensor_kit : このリポジトリは、起動ファイル、そのパイプライン構成、およびセンサーの説明を検出するために使用されます。フォークして autoware メタリポジトリとして名前を変更してください。この時点で、フォークされたリポジトリ名は になりますtutorial_vehicle_sensor_kit_launch。
Sample_vehicle_launch : このリポジトリは、車両起動ファイル、vehicle_descriptions および vehicle_model に使用されます。このリポジトリもフォークして名前を変更してください。この時点で、フォークされたリポジトリ名は になりますtutorial_vehicle_launch。
autoware_individual_params : このリポジトリには、各車両に応じて変化するパラメータ (つまり、センサーのキャリブレーション) が保存されます。このリポジトリもフォークして名前を変更してください。フォークされたリポジトリ名は になりますtutorial_vehicle_individual_params。
autoware_launch : このリポジトリには、Autoware のノード構成とそのパラメータが含まれています。フォークして、以前にフォークしたリポジトリと同じ名前に変更してください。フォークされたリポジトリ名は になりますautoware_launch.tutorial_vehicle。
2. 環境に合わせて autoware.repos をカスタマイズする
フォークされたリポジトリをインポートするには、をカスタマイズする必要がありますautoware.repos。通常、このautoware.reposファイルには、必要なすべての Autoware リポジトリ (キャリブレーション リポジトリとシミュレータ リポジトリを除く) の情報が含まれています。したがって、フォークされたリポジトリもこのファイルに追加する必要があります。

2.1 autoware.repos への個々のリポジトリの追加
autoware.reposすべてのリポジトリをフォークした後、任意のテキスト エディタを使用してファイルを開き、独自の個別のリポジトリでsample_sensor_kit_launch、 sample_vehicle_launch、autoware_individual_params および を更新することで、Autoware メタ リポジトリへの追加を開始できます。autoware launchたとえば、このチュートリアルでは、フォークされたtutorial_vehicleリポジトリに必要な変更は次のようになります。

センサーキット:

- sensor_kit/sample_sensor_kit_launch:
-   type: git
-   url: https://github.com/autowarefoundation/sample_sensor_kit_launch.git
-   version: main
+ sensor_kit/tutorial_vehicle_sensor_kit_launch:
+   type: git
+   url: https://github.com/leo-drive/tutorial_vehicle_sensor_kit_launch.git
+   version: main
車両の打ち上げ:

- vehicle/sample_vehicle_launch:
-   type: git
-   url: https://github.com/autowarefoundation/sample_vehicle_launch.git
-   version: main
+ vehicle/tutorial_vehicle_launch:
+   type: git
+   url: https://github.com/leo-drive/tutorial_vehicle_launch.git
+   version: main
個々のパラメータ:

- param/autoware_individual_params:
-   type: git
-   url: https://github.com/autowarefoundation/autoware_individual_params.git
-   version: main
+ param/tutorial_vehicle_individual_params:
+   type: git
+   url: https://github.com/leo-drive/tutorial_vehicle_individual_params.git
+   version: main
オートウェアの起動:

- launcher/autoware_launch:
-   type: git
-   url: https://github.com/autowarefoundation/autoware_launch.git
-   version: main
+ launcher/autoware_launch.tutorial_vehicle:
+   type: git
+   url: https://github.com/leo-drive/autoware_launch.tutorial_vehicle.git
+   version: main
独自の autoware.repos ファイルにも同様の変更を加えてください。これらの変更を加えた後、VCS を使用して必要なすべてのリポジトリを Autoware ワークスペースにインポートする準備が整います。

まず、独自の Autoware メタリポジトリ ディレクトリの下に src ディレクトリを作成します。

cd <YOUR-AUTOWARE-DIR>
mkdir src
次に、必要なすべてのリポジトリを vcs でインポートします。

cd <YOUR-AUTOWARE-DIR>
vcs import src < autoware.repos
コマンドを実行すると、すべての Autoware リポジトリがAutoware ディレクトリの下のフォルダvcs importに複製されます。src

これで、colcon build コマンドを使用して独自のリポジトリを構築できます。

cd <YOUR-AUTOWARE-DIR>
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
車両の各パッケージを作成およびカスタマイズする方法については、次のドキュメントのリンクを参照してください。

車両とセンサーのモデルの作成
センサーモデルの作成
個別のパラメータの作成
車両モデルの作成
車両インターフェースパッケージの作成
ディファレンシャルドライブモデルのカスタマイズ
autoware.reposパッケージが Autoware リポジトリ内で適切に含まれ、管理されていることを確認するために、インターフェイスや説明などのすべてのカスタム パッケージを忘れずに追加してください。
# Creating Autoware repositories

## What is a Meta-repository?

A meta-repository is a repository that manages multiple repositories,
and [Autoware](https://github.com/autowarefoundation/autoware) is one of them.
It serves as a centralized control point for referencing, configuring,
and versioning other repositories.
To accomplish this,
the Autoware meta-repository includes the [`autoware.repos`](https://github.com/autowarefoundation/autoware/blob/main/autoware.repos) file
for managing multiple repositories.
We will use the [VCS](https://github.com/dirk-thomas/vcstool) tool
(Version Control System) to handle the .repos file.
VCS provides us with the capability to import, export, and pull from multiple repositories.
VCS will be used to import all the necessary repositories to build Autoware into our workspace.
Please refer to the documentation for VCS and .repos file usage.

## How to create and customize your autoware meta-repository

### 1. Create autoware meta-repository

If you want to integrate Autoware into your vehicle, the first step is to create an Autoware meta-repository.

One easy way is to fork the Autoware repository and clone it.
(For instructions on how to fork a repository,
refer to [GitHub Docs](https://docs.github.com/en/get-started/quickstart/fork-a-repo))

- In this guide, Autoware will be integrated into a `tutorial_vehicle`
  (Note: when setting up multiple types of vehicles,
  adding a suffix like `autoware.vehicle_A` or `autoware.vehicle_B` is recommended).
  For the first step,
  please visit the [autoware](https://github.com/autowarefoundation/autoware) repository
  and click the fork button.
  The fork process should look like this:

<figure markdown>
  ![forking-autoware_repository](images/forking-autoware_repository.png){ align=center }
  <figcaption>
    Sample forking demonstration for tutorial_vehicle
  </figcaption>
</figure>

Then click "Create fork" button to continue. After that, we can clone our fork repository on our local system.

```bash
git clone https://github.com/YOUR_NAME/autoware.<YOUR-VEHICLE>.git
```

For example, it should be for our documentation:

```bash
git clone https://github.com/leo-drive/autoware.tutorial_vehicle.git
```

#### 1.1 Create vehicle individual repositories

To integrate Autoware into your individual vehicles,
you need to fork and modify the following repositories as well:

- [sample_sensor_kit](https://github.com/autowarefoundation/sample_sensor_kit_launch): This repository will be used for sensing launch files, their pipelines-organizations and sensor descriptions.
  Please fork and rename as autoware meta-repository. At this point, our forked repository name will be `tutorial_vehicle_sensor_kit_launch`.
- [sample_vehicle_launch](https://github.com/autowarefoundation/sample_vehicle_launch): This repository will be used for vehicle launch files, vehicle_descriptions and vehicle_model.
  Please fork and rename this repository as well. At this point, our forked repository name will be `tutorial_vehicle_launch`.
- [autoware_individual_params](https://github.com/autowarefoundation/autoware_individual_params): This repository stores parameters that change depending on each vehicle (i.e. sensor calibrations). Please fork
  and rename this repository as well; our forked repository name will be `tutorial_vehicle_individual_params`.
- [autoware_launch](https://github.com/autowarefoundation/autoware_launch):
  This repository contains node configurations and their parameters for Autoware.
  Please fork and rename it as the previously forked repositories;
  our forked repository name will be `autoware_launch.tutorial_vehicle`.

### 2. Customize autoware.repos for your environment

You need to customize your `autoware.repos` to import your forked repositories.
The `autoware.repos` file usually includes information for all necessary Autoware repositories
(except calibration and simulator repositories).
Therefore, your forked repositories should also be added to this file.

#### 2.1 Adding individual repos to autoware.repos

After forking all repositories,
you can start
adding them to your Autoware meta-repository
by opening the `autoware.repos` file using any text editor and updating `sample_sensor_kit_launch`,
`sample_vehicle_launch`, `autoware_individual_params`
and `autoware launch` with your own individual repos.
For example, in this tutorial,
the necessary changes for our forked `tutorial_vehicle` repositories should be as follows:

- Sensor Kit:

  ```diff
  - sensor_kit/sample_sensor_kit_launch:
  -   type: git
  -   url: https://github.com/autowarefoundation/sample_sensor_kit_launch.git
  -   version: main
  + sensor_kit/tutorial_vehicle_sensor_kit_launch:
  +   type: git
  +   url: https://github.com/leo-drive/tutorial_vehicle_sensor_kit_launch.git
  +   version: main
  ```

- Vehicle Launch:

  ```diff
  - vehicle/sample_vehicle_launch:
  -   type: git
  -   url: https://github.com/autowarefoundation/sample_vehicle_launch.git
  -   version: main
  + vehicle/tutorial_vehicle_launch:
  +   type: git
  +   url: https://github.com/leo-drive/tutorial_vehicle_launch.git
  +   version: main
  ```

- Individual Params:

  ```diff
  - param/autoware_individual_params:
  -   type: git
  -   url: https://github.com/autowarefoundation/autoware_individual_params.git
  -   version: main
  + param/tutorial_vehicle_individual_params:
  +   type: git
  +   url: https://github.com/leo-drive/tutorial_vehicle_individual_params.git
  +   version: main
  ```

- Autoware Launch:

  ```diff
  - launcher/autoware_launch:
  -   type: git
  -   url: https://github.com/autowarefoundation/autoware_launch.git
  -   version: main
  + launcher/autoware_launch.tutorial_vehicle:
  +   type: git
  +   url: https://github.com/leo-drive/autoware_launch.tutorial_vehicle.git
  +   version: main
  ```

Please make similar changes to your own autoware.repos file.
After making these changes,
you will be ready to use VCS to import all the necessary repositories into your Autoware workspace.

First, create a src directory under your own Autoware meta-repository directory:

```bash
cd <YOUR-AUTOWARE-DIR>
mkdir src
```

Then, import all necessary repositories with vcs:

```bash
cd <YOUR-AUTOWARE-DIR>
vcs import src < autoware.repos
```

After the running `vcs import` command,
all autoware repositories will be cloned in the `src` folder under the Autoware directory.

Now, you can build your own repository with colcon build command:

```bash
cd <YOUR-AUTOWARE-DIR>
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

Please refer to the following documentation links for instructions
on how to create and customize each of your vehicle's packages:

- [Creating vehicle and sensor models](../creating-vehicle-and-sensor-model/index.md)
  - [Creating sensor model](../creating-vehicle-and-sensor-model/creating-sensor-model/index.md)
  - [Creating individual params](../creating-vehicle-and-sensor-model/creating-individual-params/index.md)
  - [Creating vehicle model](../creating-vehicle-and-sensor-model/creating-vehicle-model/index.md)
- [creating-vehicle-interface-package](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/integrating-autoware/creating-vehicle-interface-package/creating-a-vehicle-interface-for-an-ackermann-kinematic-model/)
- [customizing-for-differential-drive-model](https://autowarefoundation.github.io/autoware-documentation/main/how-to-guides/integrating-autoware/creating-vehicle-interface-package/customizing-for-differential-drive-model/)

Please remember to add all your custom packages, such as interfaces and descriptions, to your `autoware.repos` to ensure that your packages are properly included and managed within the Autoware repository.
