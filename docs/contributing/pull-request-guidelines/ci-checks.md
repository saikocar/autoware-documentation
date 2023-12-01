# CIチェック

Autoware には、プルリクエストに対していくつかのチェックがあります。
結果は以下のようにプルリクエストページの下部に表示されます。

![ci-checks](images/ci-checks.png)

❌マークが表示されている場合は、`Details`ボタンをクリックして失敗の原因を調査してください。

`Required`マークが表示されている場合は、エラーを解決しない限りプルリクエストをマージできません。
そうでない場合はオプションですが、できれば修正する必要があります。

次のセクションではAutowareの一般的なCIチェックについて説明します。  
一部のリポジトリでは設定が異なる場合があることに注意してください。

## DCO

開発者証明書(DCO)は、貢献者がプロジェクトに貢献しているコードを作成したこと、またはそのコードを提出する権利を持っていることを証明するための軽量な方法です。

このワークフローは、プルリクエストが`DCO`を満たすかどうかをチェックします。  
[必要な項目](https://developercertificate.org/)を確認して`git commit -s`でコミットする必要があります。

詳細については[GitHubアプリページ](https://github.com/apps/dco)を参照してください。

## セマンティックプルリクエスト

このワークフローはプルリクエストが[従来のコミット](https://www.conventionalcommits.org/en/v1.0.0/)に従っているかどうかをチェックします。

詳しいルールについては[プルリクエストのルール](index.md#pull-request-rules)をご覧ください。

## 事前コミット

[pre-commit](https://pre-commit.com/)はコミット時にフォーマッタまたはリンターを実行するツールです。

このワークフローは、プルリクエストに`pre-commit`でエラーがないかどうかをチェックします。

ワークフローでは、リポジトリで`pre-commit.ci - pr`が有効になっており、[pre-commit.ci](https://pre-commit.ci/)によって可能な限り多くのエラーが自動的に修正されます。  
エラーが残っている場合は、手動で修正してください。

次のコマンドを使用して、ローカル環境で`pre-commit`を実行できます:

```bash
pre-commit run -a
```

もしくは`pre-commit`をリポジトリにインストールし、コミット前に自動で実行することができます:

```bash
pre-commit install
```

誤検知のないエラーを検出することは難しいため、一部のジョブは別の構成ファイルに分割され、オプションとしてマークされています。  
それらを確認するには、`--config`オプションを使用します:

```bash
pre-commit run -a --config .pre-commit-config-optional.yaml
```

## スペルチェックの差分

このワークフローは[辞書ファイル](https://github.com/tier4/autoware-spell-check-dict/blob/main/.cspell.json)を用いた[CSpell](https://github.com/streetsidesoftware/cspell)とを使用してスペルミスを検出します。
誤検知のないエラーを検出することは難しいため、オプションのワークフローですが、スペルミスはできるだけ削除することが望ましいです。

辞書に登録されていない単語を使用する場合は、以下の選択肢があります。

- その単語が少数のファイルでのみ使用されている場合は、[インライン文書設定 "cspell:ignore"](https://cspell.org/configuration/document-settings/)を使用してチェックを抑制できます。
- その単語がリポジトリ内で広く使用されている場合は、ローカルのcspell jsonを作成し、それを[スペルチェックアクション](https://github.com/autowarefoundation/autoware-github-actions/tree/main/spell-check)に渡すことができます。
- 単語が一般的で、多くのリポジトリで使用される可能性がある場合は、[tier4/autoware-spell-check-dict](https://github.com/tier4/autoware-spell-check-dict)または[tier4/cspell-dicts](https://github.com/tier4/cspell-dicts)にプルリクエストを送信して、辞書を更新できます。

## ビルドとテストの差分

このワークフローは、プル リクエストの`colcon build`と`colcon test`をチェックします。  
CIを高速化するために、すべてのパッケージをチェックするのではなく、変更されたパッケージと依存関係のみをチェックします。

## 自己ホスト型のビルドとテストの差分

このワークフローは`build-and-test-differential`の`ARM64`バージョンです。  
このワークフローを実行するには`ARM64`ラベルを追加する必要があります。

参考までに、ARMマシンはGitHubホストランナーではサポートされていないため、AWFが用意したセルフホストランナーを使用します。 
セルフホストランナーの詳細については[GitHubドキュメント](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners)を参照してください。

## デプロイドキュメント

このワークフローは、プルリクエストのプレビュードキュメントサイトをデプロイします。  
このワークフローを実行するには、`deploy-docs`ラベルを追加する必要があります。
