# C++

!!! 警告

    構築中

## 参考

このページでルールが定義されていない場合は、以下のガイドラインに従ってください。

1. <https://docs.ros.org/en/humble/Contributing/Code-Style-Language-Versions.html>
2. <https://www.autosar.org/fileadmin/standards/adaptive/22-11/AUTOSAR_RS_CPP14Guidelines.pdf>
3. <https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines>

また、各ファイルにClang-Tidyを適用することをお勧めします。
使用方法については[ROSパッケージへのClang-Tidyの適用](../../../how-to-guides/others/applying-clang-tidy-to-ros-packages.md)を参照してください。

Clang-Tidyではすべてのルールがカバーされているわけではないことに注意してください。

## スタイルルール

### 定義された順序でヘッダーファイルをインクルードする (必須、部分的に自動化)

#### 理論的根拠

- ヘッダーの順序が異なる場合、間接的な依存関係によりC++のインクルードシステムは異なる動作をします。
- 意図しないバグを減らすにはローカルヘッダーファイルを最初に配置する必要があります。

#### 参考

- <https://llvm.org/docs/CodingStandards.html#include-style>

#### 例

次の順序でヘッダーをインクルードします:

- メインモジュールのヘッダー
- ローカルパッケージのヘッダー
- その他のパッケージのヘッダー
- メッセージヘッダーMessage headers
- Boostヘッダー
- Cのシステムヘッダー
- C++のシステムヘッダー

```cpp
// 準拠
#include "my_header.hpp"

#include "my_package/foo.hpp"

#include <package1/foo.hpp>
#include <package2/bar.hpp>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>
```

""と<>を適切に使用すると、事前に設定したClangFormatはヘッダーを自動的にソートします。

自動ソートが妨げられるため、`#include`行の間にマクロを定義しないでください。

```cpp
// 非準拠
#include <package1/foo.hpp>
#include <package2/bar.hpp>

#define EIGEN_MPL2_ONLY
#include "my_header.hpp"
#include "my_package/foo.hpp"

#include <Eigen/Core>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>
```

代わりに`#include`行の前にマクロを定義します。

```cpp
// 準拠
#define EIGEN_MPL2_ONLY

#include "my_header.hpp"

#include "my_package/foo.hpp"

#include <Eigen/Core>
#include <package1/foo.hpp>
#include <package2/bar.hpp>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>
```

特定の位置にマクロを定義する理由がある場合は、マクロの前にコメントを記述します。

```cpp
// 準拠
#include "my_header.hpp"

#include "my_package/foo.hpp"

#include <package1/foo.hpp>
#include <package2/bar.hpp>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>

// For the foo bar reason, the FOO_MACRO must be defined here.
#define FOO_MACRO
#include <foo/bar.hpp>
```

### 関数名には小文字のスネークケースを使用します(必須、部分的に自動化)

#### 理論的根拠

- これはC++標準ライブラリと一致しています。
- PythonやRustなどの他のプログラミング言語と一貫性があります。
#### 例外

- Qtなどの外部プロジェクトクラスから継承したクラスのメンバー関数についてはその命名規則に従ってください。

#### 参考

- <https://docs.ros.org/en/humble/The-ROS2-Project/Contributing/Code-Style-Language-Versions.html#function-and-method-naming>

#### 例

```cpp
void function_name()
{
}
```

### 列挙名に大文字のキャメルケースを使用する(必須、部分的に自動化)

#### 理論的根拠

- ROS2コアパッケージと一貫性があります。

#### 例外

- `rosidl`ファイルで定義された列挙型では、他の命名規則を使用できます。

#### 参考

- <http://wiki.ros.org/CppStyleGuide> (Refer to "15. Enumerations")

#### 例

```cpp
enum class Color
{
  Red, Green, Blue
}
```

### 定数名には小文字のスネークケースを使用する(必須、部分的に自動化)

#### 理論的根拠

- ROS2コアパッケージと一貫性があります。
- `std::numbers`と一貫性があります。

#### 例外

- `rosidl`ファイルで定義された定数には、他の命名規則を使用できます。

#### 参考

- <https://en.cppreference.com/w/cpp/numeric/constants>

#### 例

```cpp
constexpr double gravity = 9.80665;
```

### 複合語の頭字語と短縮形を1つの単語として数える (必須、部分的に自動化)

#### 理論的根拠

- 頭字語が連続する場合に単語の境界を明確にするため。

#### 参考

- <https://rust-lang.github.io/api-guidelines/naming.html#casing-conforms-to-rfc-430-c-case>

#### 例

```cpp
class RosApi;
RosApi ros_api;
```
