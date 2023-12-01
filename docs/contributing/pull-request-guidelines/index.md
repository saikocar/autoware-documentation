# プルリクエストのガイドライン

## 一般的なプルリクエストのワークフロー

Autoware はフォークアンドプルモデルを使用します。
モデルの詳細については[GitHubドキュメント](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests)を参照してください。

以下はフォークアンドプルモデルに基づくプルリクエストワークフローの一般的な例です。
Autowareに貢献する場合は、このワークフローを参考として使用してください。

1. 問題を作成します。
   - この問題へのアプローチについてメンテナーと話し合ってください。
   - 問題を作成する前に[サポートガイドライン](../../support/support-guidelines.md)を確認してください。
   - 他の寄稿者と議論するときは[ディスカッションガイドライン](../discussion-guidelines/index.md)に従ってください。
2. フォークリポジトリを作成します。 (初回のみ)
3. 問題で合意されたアプローチに従ってフォークリポジトリにコードを記述します。
   - 必要に応じてテストとドキュメントを作成します。
   - コードを記述するときは[コーディングガイドライン](../coding-guidelines/index.md)のガイドラインに従ってください。
   - テストを作成するときは[テストガイドライン](../testing-guidelines/index.md)のガイドラインに従ってください。
   - ドキュメントを作成するときは[ドキュメントガイドライン](../documentation-guidelines/index.md)のガイドラインに従ってください。
   - 変更をコミットするときは[コミットガイドライン](commit-guidelines.md)に従ってください。
4. コードをテストします。
   - 後のレビュープロセスでテスト結果を説明する必要があるため、テスト結果を要約することをお勧めします。
   - どのようなテストを実行すべきかわからない場合は、メンテナーと話し合ってください。
5. プルリクエストを作成します。
   - プルリクエストを作成するときは[プルリクエストのルール](#pull-request-rules)に従ってください。
6. プルリクエストがレビューされるまで待ちます。
   - レビュー担当者は[レビューガイドライン](review-guidelines.md)に従ってコードをレビューします。
     - 査読者だけでなく著者も査読ガイドラインを理解することが推奨されます。
   - [CIチェック](ci-checks.md)が失敗した場合は、エラーを修正します。
7. 査読者によって指摘されたレビューコメントに対処します。
   - レビューコメントの意味がわからない場合は、理解できるまでレビュー担当者に質問してください。
     - 作成者は自身のプル リクエストの最終的な内容に責任を負う必要があるため、理由を理解せずに修正することはお勧めできません。
   - レビューのコメントに同意できない場合は、レビュー担当者に合理的な理由を尋ねてください。
     - 査読者は、著者に各コメントの意味を理解させる義務があります。
   - レビューコメントの作成が完了したらレビュー担当者にレビューを再リクエストし、6.に戻ります。
   - 新しいレビューコメントがなくなった場合、レビュー担当者はプルリクエストを承認し、8.に進みます。
8. プルリクエストをマージします。
   - メンテナーからの特別なリクエストがない限り、書き込みアクセス権を持つ人は誰でもプルリクエストをマージできます。
     - 作成者は自分のプルリクエストに対する責任を感じるためにプルリクエストをマージすることをお勧めします。
     - 作成者に書き込みアクセス権がない場合は、レビュー担当者またはメンテナーに問い合わせてください。

## プルリクエストのルール

### 適切なプルリクエストテンプレートを使用します (必須、非自動化)

#### 理論的根拠

- テンプレートによる記述スタイルの統一によりレビューを効率化できます。

#### 例

テンプレートは2種類あります。以下の条件に基づいて選択してください。

1. [標準的な変更](https://github.com/autowarefoundation/autoware/blob/main/.github/PULL_REQUEST_TEMPLATE/standard-change.md):
   - 複雑さ:
     - 新機能または重要なアップデート。
     - コードベースについてのより深い理解が必要です。
   - 重大さ:
     - システムの複数の部分に影響します。
     - 基本的には、マイナーな機能、バグ修正、パフォーマンスの向上が含まれます。
     - マージする前にテストが必要です。
2. [小さな変更](https://github.com/autowarefoundation/autoware/blob/main/.github/PULL_REQUEST_TEMPLATE/small-change.md):
   - 複雑さ:
     - ドキュメント、単純なリファクタリング、またはスタイルの調整。
     - 分かりやすく復習しやすい。
   - 重大さ:
     - システムへの影響は最小限です。
     - 必要なテストが少なくなり、マージが迅速化されます。

##### 適切なプルリクエストテンプレートを使用する手順

1. [このビデオ](https://user-images.githubusercontent.com/31987104/184344710-2adee239-799f-4fdf-bfab-be76345bfac1.mp4)で示されているように、適切なテンプレートを選択します。
2. 選択したテンプレートをよく読み、必要な内容を入力します。
3. レビュー中にチェックボックスをオンにします。
   - 著者向けの[レビュー前チェックリスト](https://github.com/autowarefoundation/autoware/blob/44c70d33825617b56d8de5e6fe921000238238bd/.github/PULL_REQUEST_TEMPLATE/standard-change.md#pre-review-checklist-for-the-pr-author)と[レビュー後チェックリスト](https://github.com/autowarefoundation/autoware/blob/44c70d33825617b56d8de5e6fe921000238238bd/.github/PULL_REQUEST_TEMPLATE/standard-change.md#post-review-checklist-for-the-pr-author)があります。

### プルリクエストの作成後に適切なレビュー担当者を設定します(必須、部分的に自動化)

#### 理論的根拠

- コードベースの品質を維持するには、プルリクエストは適切なレビュー担当者によってレビューされる必要があります。

#### 例

- ほとんどのROSパッケージでは、`package.xml`内の`maintainer`情報に基づいてレビュー担当者が自動的に割り当てられます。
- レビュー担当者が自動的に割り当てられない場合は[GitHubドキュメント](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review)の指示に従ってレビュー担当者を手動で割り当てます。
  - リポジトリの`.github/CODEOWNERS`ファイルを参照すると、レビュー担当者を見つけることができます。
- 適切なレビュー担当者がわからない場合は`@autoware-maintainers`に問い合わせてください。
- レビュー担当者を割り当てる権限がない場合は、代わりにレビュー担当者に言及してください。

### 従来のコミットをプルリクエストタイトルに適用する(必須、自動)

#### 理論的根拠

- [従来のコミット](https://www.conventionalcommits.org/en/v1.0.0/)では例えば[git-cliff](https://github.com/orhun/git-cliff)を使用して、分類された変更ログを生成できます。

#### 例

```text
feat(trajectory_follower): add an awesome feature
```

!!! 注記

    説明部分 (ここでは`add an awesome feature`) は小文字で始める必要があります。

変更により一部のインターフェースが壊れる場合は次のように`!` (破壊的な変更)とマークします:

```text
feat(trajectory_follower)!: remove package
feat(trajectory_follower)!: change parameter names
feat(planning)!: change topic names
feat(autoware_utils)!: change function names
```

コードを含むリポジトリ (ほとんどのリポジトリ) の場合は、タイプとして[conventional-commit-typesの定義](https://github.com/commitizen/conventional-commit-types/blob/c3a9be4c73e47f2e8197de775f41d981701407fb/index.json)を使用します。

[autoware-documentation](https://github.com/autowarefoundation/autoware-documentation)などのドキュメントリポジトリの場合は、以下の定義を使用します:

- `feat`
  - Add new pages.
  - Add contents to the existing pages.
- `fix`
  - Fix the contents in the existing pages.
- `refactor`
  - Move contents to different pages.
- `docs`
  - Update documentation for the documentation repository itself.
- `build`
  - Update the settings of the documentation site builder.
- `!` (breaking changes)
  - Remove pages.
  - Change the URL of pages.

`perf`と`test`は通常は使用されません。
他のタイプはコードリポジトリと同じ意味を持ちます。

### 関連するコンポーネント名を従来のコミットの範囲に追加します(推奨、非自動化)

#### 理論的根拠

- これは寄稿者が自分に関連するプルリクエストを見つけるのに役立ちます。
- 変更ログがより明確になります。

#### 例

ROSパッケージの場合は、パッケージ名またはコンポーネント名を追加すると良いでしょう。

```text
feat(trajectory_follower): add an awesome feature
refactor(planning, control): use common utils
```

### プルリクエストは小さくしてください(推奨、非自動化)

#### 理論的根拠

- 小さなプルリクエストはレビュー担当者にとって理解しやすいです。
- 小規模なプルリクエストは、メンテナにとって簡単に元に戻すことができます。

#### 例外

大きなプルリクエストを送信する以外に方法がないことがメンテナと合意されている場合は許容されます。

#### 例

- 1つのプルリクエストで2つの機能を開発することは避けてください。
- 同じコミット内に異なるタイプの変更 (`feat`, `fix`, `refactor`など) を混在させないでください。

### 1週間以上応答がない場合はレビュー担当者に通知します(推奨、非自動)

#### 理論的根拠

- マージされるまで自分のプルリクエストを管理するのは作成者の責任です。

#### 例

```text
@{some-of-developers} このPRを確認していただけますか?
@autoware-maintainers 確認しました。
```
