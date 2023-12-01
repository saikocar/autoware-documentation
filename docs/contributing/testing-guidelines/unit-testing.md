# 単体テスト

単体テストはテストの最初のフェーズであり、クラスや関数などのソースコードの単位を検証するために使用されます。
通常、コードのユニットはさまざまな入力に対する出力を検証することによってテストされます。
単体テストはコードが意図したとおりに動作することを確認し、誤って動作が変更されるのを防ぐのに役立ちます。

Autowareは、`ament_cmake`フレームワークを使用してテストを構築および実行します。
テスト結果の分析にも同じフレームワークが使用されます。

`ament_cmake`は、CMakeベースのパッケージにテストを簡単に登録し、JUnit互換の結果ファイルが生成されるようにするための便利な関数をいくつか提供します。
現在`pytest`、`gtest`、`gmock`などのいくつかの異なるテストフレームワークをサポートしています。

ROSトピックのパブリッシュおよびサブスクライブ時に並行して実行されるテストが相互に干渉するのを防ぐために[`ament_cmake_ros`](https://github.com/ros2/ament_cmake_ros/tree/master/ament_cmake_ros/cmake)のコマンドを使用してテストを分離して実行することをお勧めします。

`ament_add_ros_isolated_gtest`を`colcon test`で使用する例については以下を参照してください。
他のすべてのテストも同様のパターンに従います。

## gtestで単体テストを作成する

`my_cool_pkg/test`で、`gtest`コードファイル`test_my_cool_pkg.cpp`を作成します:

```cpp
#include "gtest/gtest.h"
#include "my_cool_pkg/my_cool_pkg.hpp"
TEST(TestMyCoolPkg, TestHello) {
  EXPECT_EQ(my_cool_pkg::print_hello(), 0);
}
```

`package.xml`に次の行を追加します:

```xml
<test_depend>ament_cmake_ros</test_depend>
```

次に`CMakeLists.txt`の`BUILD_TESTING`の下にエントリを追加して、テストソースファイルをコンパイルします:

```cmake
if(BUILD_TESTING)

  ament_add_ros_isolated_gtest(test_my_cool_pkg test/test_my_cool_pkg.cpp)
  target_link_libraries(test_my_cool_pkg ${PROJECT_NAME})
  target_include_directories(test_my_cool_pkg PRIVATE src)  # For private headers.
...
endif()
```

これによりテストが`gtest`によって提供されるデフォルトのmain関数に自動的にリンクされます。
テスト対象のコードは通常、別のCMakeターゲット(この例では`${PROJECT_NAME}`)にあり、リンク用の共有オブジェクトを追加する必要があります。
テストソースファイルに`src`ディレクトリのプライベート ヘッダーが含まれている場合は、`target_include_directories()`関数を使用してディレクトリをインクルード パスに追加する必要があります。

新しい`gtest`項目を登録するには、テストコードをマクロ`TEST ()`でラップします。
`TEST ()`は、最終的なテストコードの生成に役立つ定義済みマクロであり、実行に使用できる`gtest`項目も登録します。
gtestはテスト実行可能ファイルの作成時にフィクスチャ名とクラスケース名の間にアンダースコアを挿入するため、テストケース名はキャメルケースである必要があります。

`gtest/gtest.h`には、`ASSERT_TRUE(condition)`、`ASSERT_FALSE(condition)`、`ASSERT_EQ(val1,val2)`、`ASSERT_STREQ(str1,str2)`、`EXPECT_EQ()`などの`gtest`の事前定義マクロも含まれています。条件が満たされていない場合、`EXPECT_*`はテストを失敗としてマークしますが、次のテスト条件に進みます。

!!! 情報

    `gtest`とその機能の詳細については[gtestリポジトリ](https://github.com/google/googletest)を参照してください。

デモ`CMakeLists.txt`の`ament_add_ros_isolated_gtest`は、`gtest`コードの追加を簡素化する`ament_cmake_ros`の事前定義マクロです。
詳細は[ament_add_gtest.cmake](https://github.com/ros2/ament_cmake_ros/tree/master/ament_cmake_ros/cmake)で確認できます。

## ビルドテスト

<!-- cspell:ignore Testfile -->

デフォルトでは必要なテストファイル(`ELF`、`CTestTestfile.cmake`など)はすべて`colcon`によってコンパイルされます:

```console
cd ~/workspace/
colcon build --packages-select my_cool_pkg
```

テストファイルは`~/workspace/build/my_cool_pkg`に生成されます。

## テストの実行

特定のパッケージのすべてのテストを実行するには次を呼び出します。:

```console
$ colcon test --packages-select my_cool_pkg

Starting >>> my_cool_pkg
Finished <<< my_cool_pkg [7.80s]

Summary: 1 package finished [9.27s]
```

テストコマンドの出力には、すべてのテスト結果の簡単なレポートが含まれています。

実行されたすべてのテストのジョブごとの情報を取得するには次を呼び出します:

```console
$ colcon test-result --all

build/my_cool_pkg/test_results/my_cool_pkg/copyright.xunit.xml: 8 tests, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/cppcheck.xunit.xml: 6 tests, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/lint_cmake.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/my_cool_pkg_exe_integration_test.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml: 1 test, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/xmllint.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped

Summary: 18 tests, 0 errors, 0 failures, 0 skipped
```

`~/workspace/log/test_<date>/<package_name>`ディレクトリで、すべての生のテストコマンド、`std_out`、および`std_err`を探します。
最新のパッケージレベルのビルドおよびテスト出力へのシンボリックリンクを含む`~/workspace/log/latest_*/`ディレクトリもあります。

テストの実行中にテストの詳細を出力するには、`--event-handlers console_cohesion+`オプションを使用して詳細をコンソールに直接出力します:

```console
$ colcon test --event-handlers console_cohesion+ --packages-select my_cool_pkg

...
test 1
    Start 1: test_my_cool_pkg

1: Test command: /usr/bin/python3 "-u" "~/workspace/install/share/ament_cmake_test/cmake/run_test.py" "~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml" "--package-name" "my_cool_pkg" "--output-file" "~/workspace/build/my_cool_pkg/ament_cmake_gtest/test_my_cool_pkg.txt" "--command" "~/workspace/build/my_cool_pkg/test_my_cool_pkg" "--gtest_output=xml:~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml"
1: Test timeout computed to be: 60
1: -- run_test.py: invoking following command in '~/workspace/src/my_cool_pkg':
1:  - ~/workspace/build/my_cool_pkg/test_my_cool_pkg --gtest_output=xml:~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from test_my_cool_pkg
1: [ RUN      ] test_my_cool_pkg.test_hello
1: Hello World
1: [       OK ] test_my_cool_pkg.test_hello (0 ms)
1: [----------] 1 test from test_my_cool_pkg (0 ms total)
1:
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1: -- run_test.py: return code 0
1: -- run_test.py: inject classname prefix into gtest result file '~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml'
1: -- run_test.py: verify result file '~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml'
1/5 Test #1: test_my_cool_pkg ...................   Passed    0.09 sec

...

100% tests passed, 0 tests failed out of 5

Label Time Summary:
copyright     =   0.49 sec*proc (1 test)
cppcheck      =   0.20 sec*proc (1 test)
gtest         =   0.05 sec*proc (1 test)
lint_cmake    =   0.18 sec*proc (1 test)
linter        =   1.34 sec*proc (4 tests)
xmllint       =   0.47 sec*proc (1 test)

Total Test time (real) =   7.91 sec
...
```

## コードカバレッジ

大まかに説明するとコードカバレッジメトリックとはテスト中にどの程度のプログラムコードが実行された (カバーされた) かを示す尺度です。

Autowareリポジトリでは[Codecov](https://app.codecov.io/gh/autowarefoundation/autoware.universe/)を使用してオープンなプルリクエストのカバレッジを自動的に計算します。

コードカバレッジメトリックの詳細については[Codecovのドキュメント](https://docs.codecov.com/docs/about-code-coverage)を参照してください。
