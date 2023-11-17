# ソースインストール

## 前提条件

- OS

  - [Ubuntu 22.04](https://releases.ubuntu.com/22.04/)

- ROS

  - ROS 2 Humble

  ROS2システムの依存関係については[REP-2000](https://www.ros.org/reps/rep-2000.html)を参照してください。

- [Git](https://git-scm.com/)
  - [SSHキーをGitHubに登録すること](https://github.com/settings/keys)を推奨します。

```bash
sudo apt-get -y update
sudo apt-get -y install git
```

> 注意:Ubuntu 20.04でROS2 Galacticを使用したい場合は、[galactic](https://autowarefoundation.github.io/autoware-documentation/galactic/installation/autoware/source-installation/) ブランチのインストール手順を参照してください。ただしGalactic版には最新の機能が含まれていない可能性があることに注意してください。

## 開発環境のセットアップ

1. `autowarefoundation/autoware`のクローンを作成し、ディレクトリに移動します。

   ```bash
   git clone https://github.com/autowarefoundation/autoware.git
   cd autoware
   ```

2. Autowareを初めてインストールする場合は提供されたAnsibleスクリプトを使用して依存関係を自動的にインストールできます。

   ```bash
   ./setup-dev-env.sh
   ```

   ビルドの問題が発生した場合は[トラブルシューティング](../../support/troubleshooting/index.md#build-issues)セクションを参照してサポートを求めてください。

!!! 注意

    NVIDIAライブラリをインストールする前にnvidiaライセンスに同意していることを確認してください。

    - [CUDA](https://docs.nvidia.com/cuda/eula/index.html)
    - [cuDNN](https://docs.nvidia.com/deeplearning/cudnn/sla/index.html)
    - [TensorRT](https://docs.nvidia.com/deeplearning/tensorrt/sla/index.html)

!!! 注記

    以下の項目が自動的にインストールされます。ansibleスクリプトが機能しない場合、または別のバージョンの依存ライブラリがすでにインストールされている場合は、以下のアイテムを手動でインストールしてください。

    - [ROS2のインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/ros2#manual-installation)
    - [ROS2 Dev Toolsのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/ros2_dev_tools#manual-installation)
    - [RMW Implementationのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/rmw_implementation#manual-installation)
    - [pacmodのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/pacmod#manual-installation)
    - [Autoware Coreの依存関係のインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/autoware_core#manual-installation)
    - [Autoware Universeの依存関係のインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/autoware_universe#manual-installation)
    - [pre-commitの依存関係のインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/pre_commit#manual-installation)
    - [Nvidia CUDAのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/cuda#manual-installation)
    - [Nvidia cuDNNとTensorRTのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/tensorrt#manual-installation)

    ansibleスクリプトを使用しなかった場合は[アーティファクトの手動ロードマニュアル](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/artifacts)で説明されているように、いくつかのパッケージアーティファクトをダウンロードする必要があります。そうしないと推論にこれらのアーティファクトが必要なため、一部のパッケージ (主に認識に関連するもの) が実行できなくなります。

## ワークスペースの設定方法

1. `src`ディレクトリを作成し、その中にリポジトリのクローンします。

   Autowareは[vcstool](https://github.com/dirk-thomas/vcstool)を使用してワークスペースを構築します。

   ```bash
   cd autoware
   mkdir src
   vcs import src < autoware.repos
   ```

2. 依存するROSパッケージをインストールします。

   Autowareにはコアコンポーネントに加えていくつかのROS2パッケージが必要です。
   `rosdep`ツールを使用すると、依存関係の自動検索とインストールが可能になります。
   `rosdep install`の前に`rosdep update`を実行する必要があります.

   ```bash
   source /opt/ros/humble/setup.bash
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

3. ワークスペースをビルドします。

   Autowareは[colcon](https://github.com/colcon)を使用してワークスペースをビルドします。
   より高度なオプションについては[ドキュメント](https://colcon.readthedocs.io/)を参照してください。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```

   ビルドで問題が発生した場合は[トラブルシューティング](../../support/troubleshooting/index.md#build-issues)を参照してください。

## ワークスペースのアップデート

1. `.repos` ファイルを更新します。

   ```bash
   cd autoware
   git pull <remote> <your branch>
   ```

   `<remote>`は通常`git@github.com:autowarefoundation/autoware.git`です。

2. リポジトリを更新します。

   ```bash
   vcs import src < autoware.repos
   vcs pull src
   ```

   Gitユーザーに向けて:

   - `vcs import`は`git checkout`と似ています。
     - remoteからは取得できないことに注意してください。
   - `vcs pull`は`git pull`と似ています.
     - ブランチは切り替わらないことに注意してください。

   詳細については[公式ドキュメント](https://github.com/dirk-thomas/vcstool)を参照してください。

3. 依存するROSパッケージをインストールします。

   ```bash
   source /opt/ros/humble/setup.bash
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

4. ワークスペースをビルドします。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```
