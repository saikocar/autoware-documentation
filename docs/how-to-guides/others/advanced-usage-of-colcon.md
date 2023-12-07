Colcon の高度な使用法
このページでは、 の高度で便利な使用法をいくつか紹介しますcolcon。さらに詳細な情報が必要な場合は、colcon のドキュメントを参照してください。

よくある間違い
ワークスペースのルート以外からは実行しないでください
colcon buildビルドは現在のディレクトリの下にのみ行われるため、常にワークスペースのルートから実行することが重要ですcolcon。誤って間違ったディレクトリを構築してしまった場合は、を実行しrm -rf build/ install/ log/て生成されたファイルをクリーンアップします。

ワークスペースを不必要にオーバーレイしないでください
colconワークスペースを構築する前に他のワークスペースのソースを取得した場合は、ワークスペースをオーバーレイしますsetup.bash。特に複数のワークスペースがある場合は、これに注意する必要があります。

実行して、echo $COLCON_PREFIX_PATHワークスペースがオーバーレイされているかどうかを確認します。一部のワークスペースが不必要にオーバーレイされている場合は、ビルドされたファイルをすべて削除し、ターミナルを再起動して環境変数をクリーンアップし、ワークスペースを再構築します。

の詳細についてはworkspace overlaying、ROS 2 のドキュメントを参照してください。

ビルドアーティファクトのクリーンアップ
colcon古いキャッシュが原因でエラーが発生することがあります。キャッシュを削除してワークスペースを再構築するには、次のコマンドを実行します。

rm -rf build/ install/
どのパッケージを削除すればよいかわかっている場合:

rm -rf {build,install}/{package_a,package_b}
ビルドするパッケージの選択
指定したパッケージのみをビルドするには:

colcon build --packages-select <package_name1> <package_name2> ...
指定したパッケージとその依存関係を再帰的にビルドするには:

colcon build --packages-up-to <package_name1> <package_name2> ...
これらのオプションは にも使用できますcolcon test。

最適化レベルの変更
DCMAKE_BUILD_TYPE最適化レベルを変更する場合に設定します。

!!! 警告

If you specify `DCMAKE_BUILD_TYPE=Debug` or no `DCMAKE_BUILD_TYPE` is given for building the entire Autoware, it may be too slow to use.
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
colcon build --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
Colcon のデフォルト設定の変更
$COLCON_HOME/defaults.yamlデフォルト設定を変更するには作成します。

mkdir -p ~/.colcon
cat << EOS > ~/.colcon/defaults.yaml
{
    "build": {
        "symlink-install": true
    }
}
詳細については、こちらを参照してください。

compile_commands.json の生成
compile_commands.json は、ビルドの依存関係とシンボルの関係を分析するために IDE/ツールによって使用されます。

フラグを使用して生成できますDCMAKE_EXPORT_COMPILE_COMMANDS=1。

colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=1
コンパイラコマンドの確認
パッケージのコンパイラとリンカーの呼び出しを確認するには、VERBOSE=1と を使用します--event-handlers console_cohesion+。

VERBOSE=1 colcon build --packages-up-to <package_name> --event-handlers console_cohesion+
その他のオプションについては、こちらを参照してください。

Cキャッシュの使用
Ccache を使用すると、再コンパイルを高速化できます。特別な理由がない限り、時間を節約するためにこれを使用することをお勧めします。

インストールCcache：

sudo apt update && sudo apt install ccache
に次のように記述します.bashrc。

export CC="/usr/lib/ccache/gcc"
export CXX="/usr/lib/ccache/g++"

# Advanced usage of colcon

This page shows some advanced and useful usage of `colcon`.
If you need more detailed information, refer to the [colcon documentation](https://colcon.readthedocs.io/).

## Common mistakes

### Do not run from other than the workspace root

It is important that you always run `colcon build` from the workspace root because `colcon` builds only under the current directory.
If you have mistakenly built in a wrong directory, run `rm -rf build/ install/ log/` to clean the generated files.

### Do not unnecessarily overlay workspaces

`colcon` overlays workspaces if you have sourced the `setup.bash` of other workspaces before building a workspace.
You should take care of this especially when you have multiple workspaces.

Run `echo $COLCON_PREFIX_PATH` to check whether workspaces are overlaid.
If you find some workspaces are unnecessarily overlaid, remove all built files, restart the terminal to clean environment variables, and re-build the workspace.

For more details about `workspace overlaying`, refer to the [ROS 2 documentation](https://docs.ros.org/en/rolling/Tutorials/Workspace/Creating-A-Workspace.html#source-the-overlay).

## Cleaning up the build artifacts

`colcon` sometimes causes errors of because of the old cache.
To remove the cache and rebuild the workspace, run the following command:

```bash
rm -rf build/ install/
```

In case you know what packages to remove:

```bash
rm -rf {build,install}/{package_a,package_b}
```

## Selecting packages to build

To just build specified packages:

```bash
colcon build --packages-select <package_name1> <package_name2> ...
```

To build specified packages and their dependencies recursively:

```bash
colcon build --packages-up-to <package_name1> <package_name2> ...
```

You can also use these options for `colcon test`.

## Changing the optimization level

Set `DCMAKE_BUILD_TYPE` to change the optimization level.

!!! warning

    If you specify `DCMAKE_BUILD_TYPE=Debug` or no `DCMAKE_BUILD_TYPE` is given for building the entire Autoware, it may be too slow to use.

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
```

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
```

## Changing the default configuration of colcon

Create `$COLCON_HOME/defaults.yaml` to change the default configuration.

```bash
mkdir -p ~/.colcon
cat << EOS > ~/.colcon/defaults.yaml
{
    "build": {
        "symlink-install": true
    }
}
```

For more details, see [here](https://colcon.readthedocs.io/en/released/user/configuration.html#defaults-yaml).

## Generating compile_commands.json

[compile_commands.json](https://colcon.readthedocs.io/en/released/user/how-to.html#cmake-packages-generating-compile-commands-json) is used by IDEs/tools to analyze the build dependencies and symbol relationships.

You can generate it with the flag `DCMAKE_EXPORT_COMPILE_COMMANDS=1`:

```bash
colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

## Seeing compiler commands

To see the compiler and linker invocations for a package, use `VERBOSE=1` and `--event-handlers console_cohesion+`:

```bash
VERBOSE=1 colcon build --packages-up-to <package_name> --event-handlers console_cohesion+
```

For other options, see [here](https://colcon.readthedocs.io/en/released/reference/event-handler-arguments.html).

## Using Ccache

[Ccache](https://ccache.dev/) can speed up recompilation.
It is recommended to use it to save your time unless you have a specific reason not to do so.

1. Install `Ccache`:

   ```bash
   sudo apt update && sudo apt install ccache
   ```

2. Write the following in your `.bashrc`:

   ```bash
   export CC="/usr/lib/ccache/gcc"
   export CXX="/usr/lib/ccache/g++"
   ```
