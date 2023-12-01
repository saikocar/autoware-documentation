# 文書化ガイドライン

!!! 独自警告

    この記事は[こちら](https://github.com/autowarefoundation/autoware-documentation/blob/main/docs/contributing/documentation-guidelines/index.md)の日本語訳であり、日本語訳のガイドラインではありません

## ワークフロー

Autowareのドキュメントへの貢献は歓迎されており、[貢献ガイドラインに記載されている事項](../index.md#pull-requests)と同じ原則に従う必要があります。小規模で限定的な変更は、このリポジトリをフォークしてプルリクエストを送信することで行うことができますが、より大きな変更についてはまずGitHubディスカッションを介してコミュニティおよびAutowareメンテナと話し合う必要があります。

小さな変更の例は以下のとおりです:

- スペルや文法の間違いを修正する
- 壊れたリンクを修正する
- [トラブルシューティング](../../support/troubleshooting/index.md)ガイドなど、明確に定義された既存のページに追加を行う。

より大きな変更の例としては、以下のようなものがあります:

- チュートリアルなど、大量の詳細情報を含む新しいページを追加する
- 既存のドキュメント構造の再編成

## スタイルガイド

可能な限り[Google開発者ドキュメントのスタイルガイド](https://developers.google.com/style)を参照する必要があります。そのガイドの[ハイライトページ](https://developers.google.com/style/highlights)を読むことをお勧めしますが、そうでない場合は以下の重要な点に注意してください。

- [標準的なアメリカ英語](https://developers.google.com/style/spelling)のスペルと句読点を使用してください。
- [大文字と小文字を区別して](https://developers.google.com/style/capitalization)文書のタイトルとセクションの見出しを記述します。
- [説明的なリンクテキストを使用します](https://developers.google.com/style/link-text).
- [短い文章](https://developers.google.com/style/translation#write-short,-clear,-and-precise-sentences)を理解と翻訳のしやすさのために記述します。

## ヒント

### 変更をプレビューする方法

ドキュメントWebサイトで変更をプレビューするには2つの方法があります。

#### 1. GitHub Actionsワークフローの使用

以下の手順に従ってください。

1. リポジトリへのプルリクエストを作成します。
2. サイドバーから`deploy-docs`ラベルを追加します(下の図を参照)。
3. 数分待つと、`github-actions`ボットがプルリクエストのプレビューのURLを通知します。

![deploy-docs-label](images/deploy-docs-label-for-pull-request.png){ width="800" }

#### 2. ローカル環境でMkDocsサーバーを実行する

PRを作成する代わりに、`mkdocs`コマンドを使用して、ローカルコンピュータ上に AutowareのドキュメントWebサイトを構築できます。 Ubuntu OS を使用していると仮定して、次のコマンドを実行して必要なライブラリをインストールします。

```bash
python3 -m pip install -U $(curl -fsSL https://raw.githubusercontent.com/autowarefoundation/autoware-github-actions/main/deploy-docs/mkdocs-requirements.txt)
```

次にドキュメントディレクトリで`mkdocs serve`を実行します。

```bash
cd /PATH/TO/YOUR-autoware-documentation
mkdocs serve
```

MkDocsサーバーが起動します。[http://127.0.0.1:8000/](http://127.0.0.1:8000/)にアクセスして、Webサイトのプレビューを表示します。
