依存パッケージのバージョンを修正する
Autoware は、依存パッケージのバージョンを autoware.repos で管理します。たとえば、autoware.universe にブランチを作成し、新しい機能を追加するとします。vcs pullautoware.universe からブランチを切り取った後、他の依存関係を更新するとします。そうなると、開発している autoware.universe のバージョンとその他の依存関係が不整合になり、Autoware ビルド全体が失敗します。開発開始時に以下のコマンドを実行して、依存パッケージのバージョンを保存することをお勧めします。

vcs export src --exact > my_autoware.repos
# Fixing dependent package versions

Autoware manages dependent package versions in autoware.repos.
For example, let's say you make a branch in autoware.universe and add new features.
Suppose you update other dependencies with `vcs pull` after cutting a branch from autoware.universe. Then the version of autoware.universe you are developing and other dependencies will become inconsistent, and the entire Autoware build will fail.
We recommend saving the dependent package versions by executing the following command when starting the development.

```bash
vcs export src --exact > my_autoware.repos
```
