# コミットガイドライン

## ブランチのルール

### ブランチ名は対応する号番号で始まります (推奨、非自動)

#### 理論的根拠

- 開発者は、対応する問題をすぐに見つけることができます。
- 道具として便利です。
- これはGitHubのデフォルトの動作と一致しています。

#### 例外

対応する問題がない場合は、このルールを無視してかまいません。

#### 例

```text
123-add-feature
```

#### 参考

- [GitHubドキュメント](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-a-branch-for-an-issue)

### ブランチ名の区切り文字にはダッシュ大文字を使用します (推奨、非自動)

#### 理論的根拠

- これはGitHubのデフォルトの動作と一致しています。

#### 例

```text
123-add-feature
```

#### 参考

- [GitHubドキュメント](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-a-branch-for-an-issue)

### ブランチ名をわかりやすいものにする (推奨、非自動化)

#### 理論的根拠

- 名前の競合を避けることができます。
- 開発者はブランチの目的を理解できます。

#### 例外

すでにプルリクエストを送信している場合は、プルリクエストを再作成する必要があるためブランチ名を変更する必要はありませんが、煩雑で時間の無駄です。  
次回からは気をつけてください。

#### 例

通常は動詞から始めるのが良いでしょう。

```text
123-fix-memory-leak-of-trajectory-follower
```

## コミットルール

### コミットのサインオフ(必須、自動)

開発者は、自分がプロジェクトに貢献しているコードを作成したか、またはそのコードを提出する権利を持っていることを証明する必要があります。

#### 理論的根拠

そうしないと、複雑なライセンス問題が発生する可能性があります。

#### 例

```bash
git commit -s
```

```text
feat: add a feature

Signed-off-by: Autoware <autoware@example.com>
```

#### 参考

- [GitHub Apps - DCO](https://github.com/apps/dco)
