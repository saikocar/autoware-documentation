# colconの高度な使用法

このページでは、`colcon`の高度で便利な使用法をいくつか紹介します。
さらに詳細な情報が必要な場合は[colconのドキュメント](https://colcon.readthedocs.io/)を参照してください。

## よくある間違い

### ワークスペースのルート以外からは実行しないでください

`colcon`ビルドは現在のディレクトリの下にのみ行われるため、常にワークスペースのルートから`colcon build`を実行することが重要です。
誤って間違ったディレクトリを構築してしまった場合は`rm -rf build/ install/ log/`を実行して生成されたファイルをクリーンアップします。

### ワークスペースを不必要にオーバーレイしないでください

ワークスペースを構築する前に他のワークスペースの`setup.bash`を実行した場合は、`colcon`はワークスペースをオーバーレイします。
特に複数のワークスペースがある場合は、これに注意する必要があります。

`echo $COLCON_PREFIX_PATH`を実行して、ワークスペースがオーバーレイされているかどうかを確認します。
一部のワークスペースが不必要にオーバーレイされている場合は、ビルドされたファイルをすべて削除し、ターミナルを再起動して環境変数をクリーンアップし、ワークスペースを再構築します。

`workspace overlaying`の詳細については[ROS2のドキュメント](https://docs.ros.org/en/rolling/Tutorials/Workspace/Creating-A-Workspace.html#source-the-overlay)を参照してください。

## ビルドアーティファクトのクリーンアップ

`colcon`は古いキャッシュが原因でエラーが発生することがあります。
キャッシュを削除してワークスペースを再構築するには、次のコマンドを実行します:

```bash
rm -rf build/ install/
```

どのパッケージを削除すればよいかわかっている場合:

```bash
rm -rf {build,install}/{package_a,package_b}
```

## ビルドするパッケージの選択

指定したパッケージのみをビルドするには:

```bash
colcon build --packages-select <package_name1> <package_name2> ...
```

指定したパッケージとその依存関係を再帰的にビルドするには:

```bash
colcon build --packages-up-to <package_name1> <package_name2> ...
```

これらのオプションは`colcon test`にも使用できます。

## 最適化レベルの変更

`DCMAKE_BUILD_TYPE`を最適化レベルを変更する場合に設定します。

!!! 警告

    `DCMAKE_BUILD_TYPE=Debug`を指定した場合、またはAutoware全体のビルドに`DCMAKE_BUILD_TYPE`が指定されていない場合、速度が遅すぎて使用できない可能性があります。

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
```

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## colconのデフォルト設定の変更

`$COLCON_HOME/defaults.yaml`デフォルト設定を変更するには`$COLCON_HOME/defaults.yaml`を作成します。

```bash
mkdir -p ~/.colcon
cat << EOS > ~/.colcon/defaults.yaml
{
    "build": {
        "symlink-install": true
    }
}
```

詳細については[こちら](https://colcon.readthedocs.io/en/released/user/configuration.html#defaults-yaml)を参照してください。

## compile_commands.jsonの生成

[compile_commands.json](https://colcon.readthedocs.io/en/released/user/how-to.html#cmake-packages-generating-compile-commands-json)は、ビルドの依存関係とシンボルの関係を分析するために IDE/ツールによって使用されます。

`DCMAKE_EXPORT_COMPILE_COMMANDS=1`フラグを使用して生成できます:

```bash
colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

## コンパイラコマンドの確認

パッケージのコンパイラとリンカーの呼び出しを確認するには、`VERBOSE=1`と`--event-handlers console_cohesion+`を使用します。:

```bash
VERBOSE=1 colcon build --packages-up-to <package_name> --event-handlers console_cohesion+
```

その他のオプションについては[こちら](https://colcon.readthedocs.io/en/released/reference/event-handler-arguments.html)を参照してください。

## Ccacheの使用

[Ccache](https://ccache.dev/)を使用すると、再コンパイルを高速化できます。
特別な理由がない限り、時間を節約するためにこれを使用することをお勧めします。

1. `Ccache`のインストール:

   ```bash
   sudo apt update && sudo apt install ccache
   ```

2. `.bashrc`に次のように記述します:

   ```bash
   export CC="/usr/lib/ccache/gcc"
   export CXX="/usr/lib/ccache/g++"
   ```
