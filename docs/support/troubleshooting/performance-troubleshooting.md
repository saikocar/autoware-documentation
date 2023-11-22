# パフォーマンスのトラブルシューティング
全体的な症状:

- Autowareの実行が予想よりも遅い
- RViz2でメッセージが遅く表示される
- 点群の遅れ
- カメラの映像が遅れている
- RViz2で点群またはマーカーがちらつく
- 複数のサブスクライバが同じパブリッシャーを使用するとメッセージレートが低下する

## 診断手順

### マルチキャストが有効になっているかどうかを確認する

#### 対象の症状

- 複数のサブスクライバが同じパブリッシャーを使用するとメッセージレートが低下する

#### 診断

マルチキャストがインターフェイスで有効になっていることを確認してください。

たとえば以下を実行すると:

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

"{your-interface-name}" is not multicast-capable: disabling multicast`というエラーメッセージが表示された場合はこれを修正する必要があります。

#### 解決法

以下のコマンドを実行しマルチキャストを許可します:

```bash
sudo ip link set multicast on {your-interface-name}
```

こうすることでDDSは意図したとおりに機能し、パフォーマンスを大幅に低下させることなく、複数のサブスクライバーが1つのパブリッシャーからデータを受信できます。

これは一時的な解決策です。コンピュータが再起動すると元に戻ります。

それを永続的にするには、

- 起動時にこれを実行するサービスを作成します (推奨)
- **あるいは** 以下の行を`~/.bashrc`ファイルに追加します:

  ```bash
  if [ ! -e /tmp/multicast_is_set ]; then
  sudo ip link set lo multicast on
  touch /tmp/multicast_is_set
  fi
  ```

  - これによりコンピュータを再起動するたびに端末でパスワードを要求される可能性があります。

### コンパイルフラグを確認する

#### 対象の症状

- Autowareの実行が予想よりも遅い
- 点群の遅れ
- 複数のサブスクライバーが同じパブリッシャーを使用すると、メッセージレートはさらに低下する。

#### 診断

`~/.bash_history`ファイルを確認して`-DCMAKE_BUILD_TYPE=Release`または`-DCMAKE_BUILD_TYPE=RelWithDebInfo`フラグがまったくない`colcon build`ディレクティブがあるかどうかを確認します。

これらのフラグを使用してビルドが開始され、同じワークスペースがこれらのフラグなしでコンパイルされた場合でも、最終的には依然として遅いビルドになります。

さらにノード、特に`pointcloud_preprocessor`ノードの動作が一般的に遅くなります。

問題の例: [issue2597](https://github.com/autowarefoundation/autoware.universe/issues/2597#issuecomment-1491789081)

#### 解決法

- メインの`autoware`フォルダ内の`build`、`install`フォルダと必要に応じて`log`フォルダを削除します。
- `Release`または`RelWithDebInfo`タグを使用してAutowareをコンパイルします:

  ```bash
  colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
  # Or build with debug flags too (comparable performance but you can debug too)
  colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
  ```

### DDS設定を確認する

#### 対象の症状

- Autowareの実行が予想よりも遅い
- RViz2でメッセージが遅く表示される
- 点群の遅れ
- カメラの映像が遅れている
- 複数のサブスクライバが同じパブリッシャーを使用するとメッセージ レートが低下する

#### RMW(ROSのミドルウェア)の実装を確認する

##### 診断

以下のコマンドを実行して使用されているミドルウェアを確認します。:

```bash
echo $RMW_IMPLEMENTATION
```

`rmw_cyclonedds_cpp`が返ってくるはずです。そうでない場合は解決法を適用します。

別のDDSミドルウェアを使用している場合はまだ正式なサポートが提供されていない可能性があります。

##### 解決法

`~/.bashrc`ファイルの独立した行に`export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp`を追加します。

#### CycloneDDSが正しく設定されているか確認する

##### 診断

以下を実行して`CycloneDDS`の`.xml`ファイルの設定を確認します:

```bash
echo $CYCLONEDDS_URI
```

戻り行は`CycloneDDS`設定を含む`.xml`ファイルを指す有効なパスである必要があります。

ファイルが正しく構成されているかどうかも確認します:

```bash
cat !{echo $CYCLONEDDS_URI}
```

`.xml`ファイルがターミナル上で表示されるはずです。

##### 解決法

[DDSの設定:DDS設定ドキュメント](../../installation/additional-settings-for-developers/index.md#tuning-dds)を確認します:

- `~/.bashrc`ファイルに`export CYCLONEDDS_URI=/absolute_path_to_your/cyclonedds_config.xml`を記述します。
- ドキュメントで提供されている設定を含む`cyclonedds_config.xml`を作成します。 

#### Linuxカーネルの最大バッファサイズを確認する

##### 診断

- `sysctl net.core.rmem_max`を実行すると少なくとも`net.core.rmem_max = 2147483647`が返されるはずです。
  - このパラメータは各ネットワーク接続の"受信バッファ"の最大サイズを指定します。これにより任意の時点でメモリに保持できるデータの最大量が決まります。最大バッファサイズを増やすことによりオペレーティングシステムはより大きなデータバーストに対応できるようになり、ネットワークの輻輳を防止しパケット損失を減らすことができ、その結果より高速で信頼性の高いデータ転送が可能になります。
- `sysctl net.ipv4.ipfrag_time`を実行すると`net.ipv4.ipfrag_time = 3`が返されるはずです。
  - "net.ipv4.ipfrag_time"パラメータはカーネルが部分的に断片化されたIPパケットを破棄するまで保持する最大時間を秒単位で指定します。このパラメータのデフォルト値は通常30秒に設定されていますが特定のオペレーティングシステムと構成によって異なる場合があります。
  - このパラメータを3秒などの低い値に設定するとカーネルは不要になった部分的に断片化されたパケットを破棄することでメモリリソースをより迅速に解放でき、システム全体のパフォーマンスと安定性の向上に役立ちます。
- `sysctl net.ipv4.ipfrag_high_thresh`を実行すると`net.ipv4.ipfrag_high_thresh = 134217728`が返されるはずです。
  - "net.ipv4.ipfrag_high_thresh"パラメータは、カーネルIPパケット再構成キュー内で許可される部分的に断片化されたパケットの数の最高基準しきい値を指定します。キュー内の部分的に断片化されたパケットの数がこのしきい値を超えると、カーネルは部分的に断片化されたパケットの数がしきい値を下回るまで、新しく到着したパケットのドロップを開始します。
  - このパラメータを134217728(128MB)などのより高い値に設定すると、カーネルはキュー内でより多くの部分的に断片化されたパケットに対応できるようになり、ファイル転送プロトコルやマルチメディアストリーミングアプリケーションのような大量のデータを転送するネットワークアプリケーションのパフォーマンスの向上に役立ちます。

これらの値の詳細な情報については[クロスベンダー調整](https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html#cross-vendor-tuning)を確認してください。

##### 解決法

いずれかを実行します:

- 以下のように`sudo touch /etc/sysctl.d/10-cyclone-max.conf`ファイルを作成します (推奨)

  - (`sudo gedit /etc/sysctl.d/10-cyclone-max.conf`)を含めるファイルを編集します:

    ```bash
    net.core.rmem_max=2147483647
    net.ipv4.ipfrag_time=3
    net.ipv4.ipfrag_high_thresh=134217728 # (128 MB)
    ```

    - コンピューターを再起動するか以下を実行して変更を有効にします:

      ```bash
      sudo sysctl -w net.core.rmem_max=2147483647
      sudo sysctl -w net.ipv4.ipfrag_time=3
      sudo sysctl -w net.ipv4.ipfrag_high_thresh=134217728
      ```

- **あるいは** 以下の行を`~/.bashrc`ファイルに記述します:

  ```bash
  if [ ! -e /tmp/kernel_network_conf_is_set ]; then
  sudo sysctl -w net.core.rmem_max=2147483647
  sudo sysctl -w net.ipv4.ipfrag_time=3
  sudo sysctl -w net.ipv4.ipfrag_high_thresh=134217728 # (128 MB)
  fi
  ```

  - これによりコンピュータを再起動するたびに端末でパスワードを要求される可能性があります。

### ROSローカルホストのみの通信が有効になっているか確認する

- マルチコンピュータセットアップを使用している場合は、このチェックをスキップしてください。
- ROSのローカルホストのみの通信を有効にするとネットワークトラフィックが削減され、ネットワーク上の他のデバイスとの潜在的な競合が回避されるため、ROSのパフォーマンスが向上します。
- [localhostのみの通信を有効にする](../../installation/additional-settings-for-developers/index.md#enabling-localhost-only-communication)も確認してください。

#### 対象の症状

- 存在すべきではないトピックが表示される
- 自分のマシンに属さない点群が表示される
  - ネットワーク上でROS2を実行している別のコンピュータからのものである可能性があります
- RViz2で点群またはマーカーがちらつく
  - 別のパブリッシャー(別のマシン上)が、ノードと同じトピックをパブリッシュしている可能性があります。
  - ちらつきの原因となります。

#### 診断

以下を実行して確認します:

```bash
echo $ROS_LOCALHOST_ONLY
```

`1`が返ってくるはずです。そうでなければ解決法を適用します。

#### 解決法

- `~/.bashrc`ファイルの独立した行に`export $ROS_LOCALHOST_ONLY=1`を加えます。
  - この環境変数は、イーサネットやWi-Fiにネットワークインターフェイスカード(NIC) を使用するのではなく、通信にループバックネットワークインターフェイス (つまりlocalhost) のみを使用するようにROSに指示します。これによりネットワークトラフィックとネットワーク上の他のデバイスとの潜在的な競合が軽減されパフォーマンスと安定性が向上します。
