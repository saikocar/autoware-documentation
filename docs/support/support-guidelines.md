# サポートのガイドライン

このページでは当社が提供するサポートの仕組みについて説明します

!!! 警告

    ヘルプを求める前にこのドキュメントサイトを検索してよく読んでください。
    また議論の際には[議論のガイドライン](../contributing/discussion-guidelines/index.md)に従ってください。

必要なヘルプの種類に応じて適切なリソースを選択し、以下のセクションの詳細な説明をお読みください。

- [ドキュメントサイト](#documentation-sites)
  - 各種情報
- [GitHubの議論](#github-discussions)
  - 質問
  - 未確認の不具合
  - 機能のリクエスト
  - デザインに関する議論
- [GitHubの問題](#github-issues)
  - 確認されている不具合
- [Discord](#discord)
  - 投稿者間のインスタントメッセージ
- [ROS談話](#ros-discourse)
  - 広く公表すべき一般的な話題

## ドキュメントサイト

[ドキュメントガイド](docs-guide.md)には役立つドキュメントサイトのリストがあります。
それらにアクセスして問題に関連する情報があるかどうかを確認してください。

ドキュメントサイトは常に最新で完璧であるとは限らないことに注意してください。
Autowareドキュメントの一部の情報が間違っている、不明瞭である、または欠落していることに気付いた場合は[貢献ガイドライン](../contributing/index.md)に従って遠慮なくプルリクエストを送信してください。

!!! 警告

    このドキュメント サイトはまだ作成中であるため、空白のページがいくつかあります。

## GitHubの議論

Autowareで問題が発生した場合は、まず既存の問題と質問を確認し、同様の問題を検索してください。

- [問題](https://github.com/autowarefoundation/autoware/issues)

  Autowareには[autoware.repos](https://github.com/autowarefoundation/autoware/blob/main/autoware.repos)にリストされている複数のリポジトリがあることに注意してください。  
  リポジトリ全体を検索することをお勧めします。

- [質問](https://github.com/autowarefoundation/autoware/discussions/categories/q-a)

回答が見つからなかった場合は[ここ](https://github.com/autowarefoundation/autoware/discussions/categories/q-a)で新しい質問スレッドを作成してください。
質問が1週間以内に回答されない場合は保守管理者に@メンションして思い出させてください。

また[機能リクエスト](https://github.com/autowarefoundation/autoware/discussions/categories/feature-requests)や[デザインに関する議論](https://github.com/autowarefoundation/autoware/discussions/categories/design)など、他の種類の議論もあります。
ご自由にそのような議論を開いたり、参加したりしてください。

ディスカッションの作成方法がわからない場合は[GitHubドキュメント](https://docs.github.com/en/discussions/quickstart#creating-a-new-discussion)を参照してください。

## GitHubの問題

問題があり、それがバグであることが確認された場合は、適切なリポジトリを見つけて、そこで新しいissueを作成します。
適切なリポジトリを判断できない場合は[Q&Aカテゴリ](https://github.com/autowarefoundation/autoware/discussions/categories/q-a)Q&Aカテゴリに新しいディスカッションを作成してメンテナに助けを求めてください。

!!! 警告

    質問や未確認のバグについては問題を作成しないでください。このような問題が発生した場合、メンテナは問題をGitHubの議論に転送します。

自分でバグを修正したい場合は、保守管理者とアプローチについて話し合い、プルリクエストを送信してください。

## Discord

[![Discord](https://img.shields.io/discord/953808765935816715?label=Join%20Autoware%20Discord&style=for-the-badge)](https://discord.gg/Q94UsPvReQ)

Autowareには、投稿者間のカジュアルなコミュニケーションのためにDiscordサーバーがあります。

Autoware Discordサーバーは以下の活動に適しています:

- コミュニティに自己紹介をする
- 寄稿者とチャットをする
- 迅速なストローポールの実施

ここは問題の解決に役立つ場所ではないことに注意してください。

## ROS談話

一般的なAutowareおよびROSコミュニティでトピックについて広く議論したい場合、またはAutowareのバグに関係のない質問をしたい場合は[ROS談話のAutowareカテゴリ](https://discourse.ros.org/c/autoware)に投稿してください。

!!! 警告

    バグに関する質問をROS談話に投稿しないでください!
