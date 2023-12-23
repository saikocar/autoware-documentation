# Dockerを利用したインストール

!!! 情報

    このページでは Docker固有の情報について説明しているため、詳細な情報が必要な場合は[ソースインストール](./source-installation.md)も併せて参照することをお勧めします。

DockerでAutowareをインストールする2つの方法は以下のとおりです:

- 1つ目の方法は事前に構築されたイメージを使用してAutowareを起動するクイックスタートです。この方法ではAutowareシミュレータの実行のみが可能でAutowareの開発はできない初心者向けです。
- 2つ目の方法は開発イメージを使用してAutowareを起動することです。これはDockerを使用したAutowareの開発と実行をサポートします。

## クイックスタート

[クイックスタート](./docker-installation-prebuilt.md)

![type:video](https://youtube.com/embed/3KUhEFkEbI8)

## 開発者向けのDockerインストール

[開発者向けのDockerインストール](./docker-installation-devel.md)

![type:video](https://youtube.com/embed/UrSF-VwncGQ)

## トラブルシューティング

特定のエラーに対する解決策は次のとおりです:

### cuda error: forward compatibility was attempted on non supported hw
### （メッセージ翻訳）cudaエラー: サポートされていないハードウェア番号で上位互換性が試行されました

NVIDIAグラフィックスのGPUサポートを有効にしてDockerを起動すると次のエラーが発生する場合があります:

```bash
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:449: container init caused \"process_linux.go:432: running prestart hook 0 caused \\\"error running hook: exit status 1, stdout: , stderr: nvidia-container-cli: initialization error: cuda error: forward compatibility was attempted on non supported hw\\\\n\\\"\"": unknown.
ERROR: Command return non-zero exit code (see above): 125
```

これは通常、新しいNVIDIAグラフィックスドライバーが(通常はapt経由で)インストールされているが、システムがまだ再起動されていないことを示します。サスペンド後の再開などの理由でグラフィックスドライバーが使用できない場合にも同様のメッセージが表示されることがあります。

これを修正するには新しいNVIDIAドライバーをインストールした後にシステムを再起動します。

### NVIDIA GPUを搭載したDockerがarm64デバイス上でAutowareを起動できない

例えばNVIDIA Jetson AGX xavierといったarm64デバイス上のNVIDIAグラフィックスに対してGPUサポートを有効にしてDockerを起動する場合、次のエラーが表示される場合があります:

```bash
nvidia@xavier:~$ rocker --nvidia --x11 --user --volume $HOME/autoware -- ghcr.io/autowarefoundation/autoware-universe:humble-latest-cuda-arm64
...

Collecting staticx==0.12.3
Downloading https://files.pythonhosted.org/packages/92/ff/d9960ea1f9db48d6044a24ee0f3d78d07bcaddf96eb0c0e8806f941fb7d3/staticx-0.12.3.tar.gz (68kB)
Complete output from command python setup.py egg_info:
Traceback (most recent call last):
File "", line 1, in
File "/tmp/pip-install-m_nm8mya/staticx/setup.py", line 4, in
from wheel.bdist_wheel import bdist_wheel
ModuleNotFoundError: No module named 'wheel'

Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-install-m_nm8mya/staticx/
...
```

このエラーはrockerツールの現在のバージョンに存在し、rockerのos_detection関数に関連しています。

このエラーを修正するにはrockerのソースコードを一時的に変更する必要がありますがお勧めできません。

現段階ではarm64デバイスではNVIDIA GPUを有効にせずにdockerを実行することをお勧めします:

```bash
rocker -e LIBGL_ALWAYS_SOFTWARE=1 --x11 --user --volume $HOME/autoware -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
```

このチュートリアルはrockerの公式修正後に更新されます。

## ヒント

### 非ネイティブのarm64システム

このセクションでは[`qemu-user-static`](https://github.com/multiarch/qemu-user-static)を使用してamd64システム上でarm64システムを実行するプロセスについて説明します。

はじめにamd64システムは通常arm64システムと互換性がありません。
以下のコマンドを実行して結果を確認します:

```sh-session
$ docker run --rm -t arm64v8/ubuntu uname -m
WARNING: The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64) and no specific platform was requested
standard_init_linux.go:228: exec user process caused: exec format error
```

qemu-user-staticをインストールするとamd64システム上でarm64イメージを実行できるようになります。

```sh-session
$ sudo apt-get install qemu-user-static
$ docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
$ docker run --rm -t arm64v8/ubuntu uname -m
WARNING: The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64) and no specific platform was requested
aarch64
```

Autowareのarm64アーキテクチャのDockerイメージを実行するにはサフィックス`-arm64`を追加します。

```sh-session
$ docker run --rm -it ghcr.io/autowarefoundation/autoware-universe:humble-latest-cuda-arm64
WARNING: The requested image's platform (linux/arm64) does not match the detected host platform (linux/amd64) and no specific platform was requested
root@5b71391ad50f:/autoware#
```
