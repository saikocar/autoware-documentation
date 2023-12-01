# 査読のヒント

## 注釈を切り替えたり、差分ビューでコメントを確認したりできます

レビュー中に差分ビューに注釈やレビューコメントが表示される場合があります。

注釈を切り替えるには`A`キーを押します。

切り替え前:

![before-press-a](images/before-press-a.png)

切り替え後:

![after-press-a](images/after-press-a.png)

レビューコメントを切り替えるには`I`キーを押します。

他のキーボード ショートカットについては[GitHubドキュメント](https://docs.github.com/en/get-started/using-github/keyboard-shortcuts)を参照してください。

## WebベースのVisual Studio Codeでコードを表示する

ブラウザーから`Visual Studio Code`を開いて、リッチなUIでコードを表示できます。
使用するには、リポジトリで`.`キーまたプルリクエストのキーはを押します。

詳しい使用方法については[github/dev](https://github.com/github/dev)を参照してください。

## プルリクエストのブランチをすぐにチェックアウトする

プル リクエストのブランチをチェックアウトしたい場合フォークアンドプルモデルでは一般に面倒です。

```bash
# Copy the user name and the fork URL.
git remote add {user-name} {fork-url}
git checkout {user-name}/{branch-name}
git remote rm {user-name} # To clean up
```

代わりに[GitHub CLI](https://cli.github.com/)を使用して手順を簡素化し`gh pr checkout {pr-number}`を実行します。

プルリクエストページの右上からコマンドをコピーできます。

![gh-pr-checkout](images/gh-pr-checkout.png)
