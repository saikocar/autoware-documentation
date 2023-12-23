# 開発向けのDockerを利用したインストール

## 要件

- [Git](https://git-scm.com/)

- NVIDIA Jetsonデバイスでは[JetPack](https://docs.nvidia.com/jetson/jetpack/install-jetpack/index.html#how-to-install-jetpack) >= 5.0がインストールされていること

## 開発環境のセットアップ

1. `autowarefoundation/autoware`をクローンしてディレクトリに移動する。

   ```bash
   git clone https://github.com/autowarefoundation/autoware.git
   cd autoware
   ```

2. 依存関係を手動あるいは提供されているAnsibleスクリプトを利用することでインストールできます。

> 注記: NVIDIAライブラリをインストールする前にライセンスを確認し同意してください。

- [CUDA](https://docs.nvidia.com/cuda/eula/index.html)

### 依存関係の手動インストール

- [Nvidia CUDAのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/cuda#manual-installation)
- [Docker Engineのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/docker_engine#manual-installation)
- [NVIDIA Container Toolkitのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/nvidia_docker#manual-installation)
- [rockerのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/rocker#manual-installation)

### Ansibleを利用した依存関係のインストール

この手法にはとても注意してください。Ansible構成を使用する前に、Ansible構成のすべての手順を必ず読んで確認してください。

依存関係を手動でインストール済みならこのセクションは省略できます。

```bash
./setup-dev-env.sh docker
```

現在のユーザーがdockerを使用できるようにするには、ログアウトしてから再度ログオンする必要がある場合があります。

## ワークスペースのセットアップ

!!! 警告

    続行する前に[NVIDIA Deep Learningコンテナライセンス](https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license)を確認し、同意してください。
    Autoware Universeイメージを取得して使用するとライセンスの利用規約に同意したことになります。

1. `autoware_map`ディレクトリを後の地図データのために作成します。

   ```bash
   mkdir ~/autoware_map
   ```

2. Dockerイメージをプルします。

   ```bash
   docker pull ghcr.io/autowarefoundation/autoware-universe:latest-cuda
   ```

3. Dockerコンテナを起動します。

   - amd64アーキテクチャのコンピュータでNVIDIA GPUを利用する場合:

     ```bash
     rocker --nvidia --x11 --user --volume $HOME/autoware --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
     ```

   - NVIDIA GPUを使わずにコンテナを走らせたい、またはarm64アーキテクチャのコンピュータの場合:

     ```bash
     rocker -e LIBGL_ALWAYS_SOFTWARE=1 --x11 --user --volume $HOME/autoware --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
     ```

     詳細な理由については[こちら](./docker-installation.md#docker-with-nvidia-gpu-fails-to-start-autoware-on-arm64-devices)で確認できます。

   高度な利用については[こちら](https://github.com/autowarefoundation/autoware/tree/main/docker/README.md)を参照してください。

   起動したらコンテナ内のワークスペースに移動します:

   ```bash
   cd autoware
   ```

4. `src`ディレクトリを作成し、リポジトリを中にクローンします。

   ```bash
   mkdir src
   vcs import src < autoware.repos
   ```

5. ROSパッケージの依存関係を更新します。

   Autowareの依存関係は、Dockerイメージの作成後に変更される可能性があります。
   その場合、以下のコマンドを実行して依存関係を更新する必要があります。

   ```bash
   sudo apt update
   rosdep update
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

6. ワークスペースを構築します。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```

   ビルドに問題がある場合は[トラブルシューティング](../../support/troubleshooting/index.md#build-issues)を参照してください。

## ワークスペースの更新

1. Dockerイメージを更新します。

   ```bash
   docker pull ghcr.io/autowarefoundation/autoware-universe:latest-cuda
   ```

2. Dockerコンテナを起動します。

   - amd64アーキテクチャのコンピュータの場合:

     ```bash
     rocker --nvidia --x11 --user --volume $HOME/autoware -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
     ```

   - NVIDIA GPUを使わずにコンテナを走らせたい、またはarm64アーキテクチャのコンピュータの場合:

     ```bash
     rocker -e LIBGL_ALWAYS_SOFTWARE=1 --x11 --user --volume $HOME/autoware -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
     ```

3. `.repos`ファイルを更新します。

   ```bash
   cd autoware
   git pull
   ```

4. リポジトリを更新します。

   ```bash
   vcs import src < autoware.repos
   vcs pull src
   ```

5. ワークスペースを構築します。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```
