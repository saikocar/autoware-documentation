# Dockerを利用したインストールのクイックスタート

## 開発環境のセットアップ

1. 依存関係の手動インストール

   - [Docker Engineのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/docker_engine#manual-installation)

   - [NVIDIA Container Toolkitのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/nvidia_docker#manual-installation)

   - [rockerのインストール](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/rocker#manual-installation)

## ワークスペースのセットアップ

1. `autoware_map`ディレクトリを後の地図データのために作成します。

   ```bash
   mkdir ~/autoware_map
   ```

2. Dockerコンテナを起動します。

   ```bash
   rocker --nvidia --x11 --user --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:humble-latest-prebuilt
   ```

   より高度な使い方については[こちら](https://github.com/autowarefoundation/autoware/tree/main/docker/README.md)を参照してください。

3. Autowareシミュレーターを実行します。

   コンテナの内部で以下のチュートリアルを実行できます:

   [計画シミュレーション](../../tutorials/ad-hoc-simulation/planning-simulation.md)

   [rosbagリプレイシミュレーション](../../tutorials/ad-hoc-simulation/rosbag-replay-simulation.md).
