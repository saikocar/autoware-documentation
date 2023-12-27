# ROSパッケージへのClang-Tidyの適用

[Clang-Tidy](https://clang.llvm.org/extra/clang-tidy/)は強力なC++のリンターです。

## 準備

Clang-Tidyを使用する前に`build/compile_commands.json`を生成する必要があります。

```bash
colcon build --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

## 使用法

```bash
clang-tidy -p build/ path/to/file1 path/to/file2 ...
```

パッケージ内のすべてのファイルにClang-Tidyを適用したい場合は[fd](https://github.com/sharkdp/fd)コマンドを使用すると便利です。
`fd`をインストールするには[インストールマニュアル](https://github.com/sharkdp/fd#on-ubuntu)を参照してください。

```bash
clang-tidy -p build/ $(fd -e cpp -e hpp --full-path "/autoware_utils/")
```

## IDEの統合

### CLion

[CLionドキュメント](https://www.jetbrains.com/help/clion/clang-tidy-checks-support.html)を参照してください。

### Visual Studio Code

次の拡張子のいずれかを使用します:

- [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- [clangd](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd)

## トラブルシューティング

`clang-diagnostic-error`が発生した場合は`libomp-dev`をインストールしてみてくださいl。

関連: <https://github.com/autowarefoundation/autoware-github-actions/pull/172>
