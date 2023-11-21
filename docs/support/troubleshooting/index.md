# トラブルシューティング

## セットアップの問題

### CUDAに関連するエラー

CUDAをインストールする際にバージョンの競合によりエラーが発生する場合があります。このようなタイプのエラーを解決するには、次のいずれかの方法を試してください:

- すべてのCUDA関連ライブラリの保留を解除し、セットアップスクリプトを再実行します。

  ```bash
  sudo apt-mark unhold  \
    "cuda*"             \
    "libcudnn*"         \
    "libnvinfer*"       \
    "libnvonnxparsers*" \
    "libnvparsers*"     \
    "tensorrt*"         \
    "nvidia*"

  ./setup-dev-env.sh
  ```

- すべてのCUDA関連ライブラリをアンインストールし、セットアップスクリプトを再実行します。

  ```bash
  sudo apt purge        \
    "cuda*"             \
    "libcudnn*"         \
    "libnvinfer*"       \
    "libnvonnxparsers*" \
    "libnvparsers*"     \
    "tensorrt*"         \
    "nvidia*"

  sudo apt autoremove

  ./setup-dev-env.sh
  ```

!!! 警告

    これによりシステムが破損する可能性があるため、慎重に実行してください。

- CUDA関連のライブラリをインストールせずにセットアップスクリプトを実行します。

  ```bash
  ./setup-dev-env.sh --no-nvidia
  ```

!!! 警告

    Autoware Universeの一部のコンポーネントにはCUDAが必要であり、現時点では[envファイル](https://github.com/autowarefoundation/autoware/blob/main/amd64.env)のCUDAバージョンのみがサポートされていることに注意してください。
    Autowareは他のCUDAバージョンでも動作する可能性がありますが、それらのバージョンはサポートされておらず機能は保証されていません。

## ビルド上の問題

### メモリの不足

Autowareのビルドには大量のメモリが必要で、ビルド中にメモリが不足するとマシンがフリーズしたりクラッシュしたりする可能性があります。この問題を回避するには16-32GB のスワップを構成する必要があります。

```bash
# Optional: Check the current swapfile
free -h

# Remove the current swapfile
sudo swapoff /swapfile
sudo rm /swapfile

# Create a new swapfile
sudo fallocate -l 32G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Optional: Check if the change is reflected
free -h
```

より詳細な構成手順とスワップの説明についてはDigital Oceanの["How To Add Swap Space on Ubuntu 20.04" tutorial](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04)を参照してください。

マシンのCPUコアが多すぎる(64個以上)場合、より大きなメモリが必要になる可能性があります。
この問題を回避するにはビルド中のジョブの数を制限します。

```bash
MAKEFLAGS="-j4" colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

`-j4`はシステムに基づいて任意の数値に調整できます。
詳細については[manual page of GNU makeのマニュアルページ](https://www.gnu.org/software/make/manual/make.html#Parallel-Disable)を確認してください.

並行してビルドされるパッケージの数を減らすことで、使用されるメモリの量も減らすことができます。
以下の例では、並行してビルドされるパッケージの数が1に設定され、`make`で使用されるジョブの数は1に制限されます。

```bash
MAKEFLAGS="-j1" colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --parallel-workers 1
```

!!! 注記

    並行してビルドされるパッケージの数と`make`で使用されるジョブの数の両方を減らすことで、メモリ使用量を減らすことができます。
    ただし、これはビルドプロセスに時間がかかることも意味します。

### 最新版のAutowareを使用している時のエラー

最新版のAutowareを使用している場合は、古いソフトウェアまたは古いビルドファイルが原因で問題が発生する可能性があります。

このようなタイプの問題を解決するには、まずビルドの成果物を削除して再構築してみてください:

```bash
rm -rf build/ install/ log/
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

エラーが解決しない場合は、`src/`を削除してインストールタイプ([Docker](../../installation/autoware/docker-installation.md#how-to-update-a-workspace) / [ソース](../../installation/autoware/source-installation.md#how-to-update-a-workspace))に応じてワークスペースをアップデートしてください。

!!! 警告

    `src/`を削除する前にローカル環境に保持したい変更がないことを確認してください!

上記の手順を試してもエラーが解決しない場合は、ワークスペース全体を削除し、もう一度リポジトリのクローンを作成して、インストール手順をやりなおしてください。

```bash
rm -rf autoware/
git clone https://github.com/autowarefoundation/autoware.git
```

### 修正版のAutowareを使用している時のエラー

修正版を使用する場合、原則としてエラーは発生しません。つまり、考えられる原因は以下のとおりです:

- ROS2が重大な変更を伴うアップデートを行った。
  - 確認のために、ROS談話の[Packaging and Release Management](https://discourse.ros.org/c/release/16)タグを確認してください。
- ローカル環境が壊れています。
  - `.bashrc`ファイル、環境変数およびライブラリのバージョンを確認します。

上記の原因に加えて修正バージョンの使用に関してよくある誤解が2つあります。

1. `autowarefoundation/autoware`修正バージョンのみ使用しました。
   完全に修正されたバージョンを使用するには`.repos`ファイル内のすべてのリポジトリバージョンを指定する必要があります。

2. `autowarefoundation/autoware`のブランチを変更した後、ワークスペースを更新しませんでした。
   `autowarefoundation/autoware`のブランチを変更しても`src/`下のファイルには影響しません。これらを更新するには`vcs import`コマンドを実行する必要があります。

### pythonパッケージのビルド中のエラー

ビルド中に以下の問題が発生する可能性があります

```bash
pkg_resources.extern.packaging.version.InvalidVersion: Invalid version: '0.23ubuntu1'
```

このエラーは66.0.0と67.5.0の間のバージョンでは`setuptools`がPythonパッケージを[PEP-440](https://peps.python.org/pep-0440/)準拠にすることを強制するために発生します。
バージョン 67.5.1 以降、`setuptools`には古いパッケージを再び操作できるようにする[フォールバック](https://github.com/pypa/setuptools/commit/1640731114734043b8500d211366fc941b741f67) があります。

解決策は、次のコマンドを使用して`setuptools`を最新バージョンに更新することです。

```bash
pip install --upgrade setuptools
```

## Docker/rockerの問題

DockerまたはrockerでAutowareを実行するときにエラーが発生した場合は、まず次のコマンドを実行して、Docker インストールが正しく動作していることを確認します:

```bash
docker run --rm -it hello-world
docker run --rm -it ubuntu:latest
```

次にGitHubパッケージWebサイトに保存されているAutowareのベースイメージにアクセスできることを確認します。

```bash
docker run --rm -it ghcr.io/autowarefoundation/autoware-universe:latest
```

## ランタイムの問題

### パフォーマンス関連の問題

症状:

- Autoware is running slower than expected
- Messages show up late in RViz2
- Point clouds are lagging
- Camera images are lagging behind
- Point clouds or markers flicker on RViz2
- When multiple subscribers use the same publishers, the message rate drops

If you have any of these symptoms, please the [Performance Troubleshooting](performance-troubleshooting.md) page.

### Map does not display when running the Planning Simulator

When running the Planning Simulator, the most common reason for the map not being displayed in RViz is because [the map path has not been specified correctly in the launch command](../../tutorials/ad-hoc-simulation/planning-simulation.md#how-to-run-a-planning-simulation). You can confirm if this is the case by searching for `Could not find lanelet map under {path-to-map-dir}/lanelet2_map.osm` errors in the log.

Another possible reason is that map loading is taking a long time due to poor DDS performance. For this, please visit the [Performance Troubleshooting](performance-troubleshooting.md) page.
