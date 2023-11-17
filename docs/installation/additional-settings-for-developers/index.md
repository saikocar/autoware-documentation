# 開発者向け追加設定

## ROS2のコンソール設定

### ロガー出力の色付け

デフォルトではROS2ロガーは出力を色付けしません。
色付けするには`.bashrc`に次のように記述します。:

```bash
export RCUTILS_COLORIZED_OUTPUT=1
```

### ロガー出力のフォーマットのカスタマイズ

デフォルトではROS2ロガーはファイル名、関数名、行番号などの詳細情報を出力しません。
カスタマイズするには`.bashrc`に次のように記述します。:

```bash
export RCUTILS_CONSOLE_OUTPUT_FORMAT="[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})"
```

その他のオプションについては[こちら](https://docs.ros.org/en/rolling/Tutorials/Logging-and-logger-configuration.html#console-output-formatting)を参照してください。

## ROS2のネットワーク設定
ROS2はDDSを採用しており、ROS2とDDSの構成については個別に説明します。
ROS2ネットワークの概念については[公式ドキュメント](http://design.ros2.org/articles/ros_on_dds.html)を参照してください。

### ROS2のネットワーク設定

ROS2はデフォルトでローカルネットワーク上にデータをマルチキャストします。
したがってオフィスで開発する場合、データはオフィスのローカルネットワーク上を流れます。
これはパケットの衝突やネットワークトラフィックの増加を引き起こす可能性があります。

これらを回避するには2つのオプションがあります。

- ローカルホストのみの通信
- ローカルネットワーク上での同一ドメインのみの通信

ローカルネットワーク上で複数のホストコンピュータを使用する予定がない限り、ローカルホストのみの通信をお勧めします。
詳細については以下のセクションを参照してください。

#### ローカルホストのみの通信を有効にする

`.bashrc`に次のように記述します。:
詳細については[ROS2のドキュメント](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html#the-ros-localhost-only-variable)を参照してください。

```bash
export ROS_LOCALHOST_ONLY=1
```

`ROS_LOCALHOST_ONLY=1`をエクスポートする場合はループバック アドレスで`MULTICAST`を有効にする必要があります。
`MULTICAST`が有効であることを確認するには次のコマンドを使用します。

```console
$ ip link show lo
1: lo: <LOOPBACK,MULTICAST,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
```

`MULTICAST`という単語が表示されない場合は、次のコマンドを使用して有効にします。

```bash
sudo ip link set lo multicast on
```

#### ローカルネットワーク上の同一ドメインのみの通信

ROS2は`ROS_DOMAIN_ID`を使用してグループを作成し、グループ内のマシン間で通信します。
すべてのROS2ノードはデフォルトでドメインID`0`を使用するため意図しない干渉が発生する可能性があります。

これを回避するには`.bashrc`内でグループごとに異なるドメインIDを設定します:

```bash
# Xを使用するドメインIDに置き換えます
# ドメインIDは[0,101]の範囲の数値である必要があります(両端の値を含む)
export ROS_DOMAIN_ID=X
```

また以下のコマンドを使用して`ROS_LOCALHOST_ONLY`が`0`であることを確認します。

```bash
echo $ROS_LOCALHOST_ONLY # 出力が1の場合localhostが優先されます。
```

詳細については[ROS2のドキュメント](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html#the-ros-domain-id-variable)を参照してください。

### DDSの設定

Autowareはノード間通信にDDSを使用します。[ROS2のドキュメント](https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html)ではDDSの機能を活用するためにDDSを調整することをユーザーに推奨しています。特に受信バッファ サイズはAutowareにとって重要なパラメータです。パラメータが十分に大きくない場合、Autowareは点群や画像などの大きなデータの受信に失敗します。

#### DDSの調整

カスタマイズしない限りデフォルトでCycloneDDSが採用されます。例としてCycloneDDSでAutowareを実行するための設定ファイルを用意します。構成ファイルのサンプルを以下に示します。`cyclonedds_config.xml`として保存します。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
<Domain Id="any">
        <General>
            <Interfaces>
                <NetworkInterface autodetermine="true" priority="default" multicast="default" />
            </Interfaces>
            <AllowMulticast>default</AllowMulticast>
            <MaxMessageSize>65500B</MaxMessageSize>
        </General>
        <Internal>
            <SocketReceiveBufferSize min="10MB"/>
            <Watermarks>
                <WhcHigh>500kB</WhcHigh>
            </Watermarks>
        </Internal>
    </Domain>
</CycloneDDS>
```

この設定は主に[Eclipse Cyclone DDS:Run-time設定ドキュメント](https://github.com/eclipse-cyclonedds/cyclonedds/tree/a10ced3c81cc009e7176912190f710331a4d6caf#run-time-configuration)から引用されています。
各値がそのように設定されている理由はドキュメントのリンクで確認できます。

Autowareを起動する前に構成ファイルのパスを設定し、Linuxカーネルの最大バッファサイズを拡大します。

```bash
export CYCLONEDDS_URI=file:///absolute/path/to/cyclonedds_config.xml
sudo sysctl -w net.core.rmem_max=2147483647
```

詳細については[ROS2のドキュメント](https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html)を参照してください。選択したDDSのユーザーガイドを読むとより深く理解できます。

#### 複数のホストコンピューター用のDDSのチューニング(上級ユーザー向け)

Autowareを複数のホストコンピュータで実行する場合はIPフラグメンテーションを考慮する必要があります。[ROS2のドキュメント](https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html#cross-vendor-tuning)で推奨されているようにIPフラグメンテーションのパラメータは次の例のように設定する必要があります。

```bash
sudo sysctl -w net.ipv4.ipfrag_time=3
sudo sysctl -w net.ipv4.ipfrag_high_thresh=134217728     # (128 MB)
```
