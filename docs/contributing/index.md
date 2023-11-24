# 貢献

貢献にご関心をお寄せいただきありがとうございます。Autowareはあなたのような人々によってサポートされており、あらゆる種類と規模の貢献を歓迎します。

寄稿者としてAutowareとその関連リポジトリに関して遵守していただきたいガイドラインを以下に示します。

- [行動規範](#code-of-conduct)
- [始める前に知っておくべきことは何ですか?](#what-should-i-know-before-i-get-started)
  - [Autowareの概念](#autoware-concepts)
  - [オープンソースプロジェクトへの貢献](#contributing-to-open-source-projects)
- [どうすれば助けが得られますか?](#how-can-i-get-help)
- [どうすれば貢献できるでしょうか?](#how-can-i-contribute)
  - [ディスカッションに参加する](#discussions)
  - [ワーキンググループに参加する](#working-groups)
  - [不具合を報告する](#bug-reports)
  - [プルリクエストを作成する](#pull-requests)

Autoware自体と同様にこれらのガイドラインは積極的に開発されており改善のための提案はいつでも歓迎されます。ガイドラインの変更は[アイデアカテゴリでディスカッションを作成すること](https://github.com/autowarefoundation/autoware/discussions/new?category=ideas)で提案できます。

## 行動規範

Autowareコミュニティがオープンかつ包括的であり続けるために[行動規範](https://github.com/autowarefoundation/autoware/blob/main/CODE_OF_CONDUCT.md)に従ってください。

コミュニティ内の誰かが行動規範に違反していると思われる場合は[conduct@autoware.org](mailto:conduct@autoware.org)に電子メールを送信して報告してください。

## 始める前に知っておくべきことは何ですか?

### Autowareの概念

Autowareのアーキテクチャと設計を高度に理解するために以下のページで簡単な概要を説明します:

- [Autoware構成要素](../design/)
- [Autowareの概念](../design/autoware-concepts/)

経験豊富な開発者の場合は[Autowareインターフェイス](../design/autoware-interfaces/)と[個々のコンポーネントページ](../design/autoware-interfaces/components/)も確認して各コンポーネントまたはモジュールの入力と出力をより詳細なレベルで理解する必要があります。

### オープンソースプロジェクトへの貢献

オープンソースプロジェクトを初めて使用する場合はGitHubの[オープンソースへの貢献方法](https://opensource.guide/how-to-contribute/)ガイドを読んで、人々がオープンソースプロジェクトに貢献する理由、貢献の意味などの概要を理解することをお勧めします。

## どうすれば助けが得られますか?

GitHubの問題は確認されたバグレポートとして保存しておきたいため、一般的なサポートの質問については問題を開かないでください。代わりにQ&Aカテゴリでディスカッションを開いてください。 Autowareのサポートメカニズムの詳細については[サポートガイドライン](../support/index.md)を参照してください。

!!! 注記

    質問または未確認のバグに対して作成された問題は、メンテナによってGitHubディスカッションに移動されます。

## どうすれば貢献できるでしょうか?

### 議論

以下のような議論を促進し、参加することでAutowareに貢献できます:

- [Autowareを強化する新機能の提案](https://github.com/orgs/autowarefoundation/discussions/categories/feature-requests)
- [既存のディスカッションに参加して意見を表明する](https://github.com/orgs/autowarefoundation/discussions)
- 他の寄稿者向けのディスカッションを企画する
- [質問に答え、他の寄稿者をサポートする](https://github.com/autowarefoundation/autoware/discussions/categories/q-a?discussions_q=category%3AQ%26A+is%3Aunanswered)

### ワーキンググループ
Autoware Foundation内の[さまざまなワーキンググループ](https://github.com/autowarefoundation/autoware-projects/wiki#working-group-list)は技術運営委員会によって設定された目標を達成する責任を負います。これらのワーキンググループは誰でも参加でき、特定のワーキンググループに参加すると、現在のプロジェクトを理解し、それらのプロジェクトが各グループ内でどのように管理されているかを確認し、特定のプロジェクトの進行に役立つ問題に貢献することができます。

今後のワーキンググループ会議のスケジュールを確認するには[Autoware Foundation のイベント カレンダー](https://calendar.google.com/calendar/u/0/embed?src=autoware.org_6lol0ho5ft0217h8c60pi1fm30@group.calendar.google.com)を参照してください。

### 不具合報告

不具合を報告する前に、問題トラッカーで適切なリポジトリを検索してください。誰かがすでに同じ問題を報告しており、回避策が存在する可能性があります。適切なリポジトリを判断できない場合は[Q&Aカテゴリ](https://github.com/autowarefoundation/autoware/discussions/new?category=q-a)に新しいディスカッションを作成してメンテナに助けを求めてください。

バグを報告する場合は、問題を再現するための最小限の手順を提供する必要があります。そうすることで、適切な問題を迅速に確認し、焦点を当てることができます。

バグを自分で修正したい場合は、それは歓迎されますが、プルリクエストを送信する前に、問題のメンテナーと考え​​られるアプローチについて話し合う必要があります。

[問題を作成するのは簡単です](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue#creating-an-issue-from-a-repository)が、問題が発生した場合は、Q&Aディスカッションを作成して助けを求めてください。

### プルリクエスト

次のような小さな変更については、プルリクエストを送信できます:

- ドキュメントのマイナーな更新
- スペルミスの修正
- 継続的統合の失敗の修正
- コンパイラまたは分析ツールによって検出された警告を修正する
- 単一のパッケージに小さな変更を加える

プルリクエストが大規模な変更である場合は、次のプロセスに従う必要があります:

1. [GitHub議論を作成](https://docs.github.com/en/discussions/collaborating-with-your-community-using-discussions/collaborating-with-maintainers-using-discussions)して変更を提案します。そうすることで、他のメンバーやAutowareメンテナーからフィードバックを得ることができ、提案された変更がAutowareの設計理念や現在の開発計画に沿っていることを確認できます。どこで会話をすればよいかわからない場合は[新しいQ&Aディスカッションを作成](https://github.com/autowarefoundation/autoware/discussions/new?category=q-a)してください。

2. 議論での合意に基づいて[問題を作成する](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue)する。

3. [プルリクエストを作成](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)して手順2で作成した課題を参照する変更を実装します。

4. 新しい追加に関するドキュメントを作成します (関連する場合)

大きな変更の例は以下のとおりです:

- Autowareに新機能を追加する
- 新しいドキュメントページまたはセクションの追加

適切なプルリクエストを送信する方法の詳細については[プルリクエストのガイドライン](pull-request-guidelines/index.md)のガイドラインを読み、必要な[ライセンス表記](license.md)を確認することを忘れないでください!
