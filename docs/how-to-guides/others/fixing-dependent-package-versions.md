# 依存パッケージのバージョンを修正する

Autowareは依存パッケージのバージョンをautoware.reposで管理します。
たとえば、autoware.universeにブランチを作成し、新しい機能を追加するとします。
`vcs pull`でautoware.universeからブランチを切り取った後、他の依存関係を更新するとします。すると、開発している autoware.universeのバージョンとその他の依存関係が不整合になり、Autowareビルド全体が失敗します。
発開始時に以下のコマンドを実行して、依存パッケージのバージョンを保存することをお勧めします。

```bash
vcs export src --exact > my_autoware.repos
```
