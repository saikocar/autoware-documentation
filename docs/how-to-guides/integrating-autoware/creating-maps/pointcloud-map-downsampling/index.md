# 点群マップのダウンサンプリング

## 概要

作成した点群マップが密すぎるか大きすぎる場合 (つまり、300 MB を超える場合)、
計算効率とメモリ効率を向上させるためにダウンサンプリングすることができます。
また、部分読み込みを伴う動的マップ読み込みの使用を検討することもできます。
詳細については、[map_loaderパッケージ](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_loader)を確認してください。

tutorial_vehicle の実装ではマップ全体を使用するため、
[CloudCompare](https://www.cloudcompare.org/main.html)を使用してダウンサンプリングします。

## CloudCompareのインストール

スナップでインストールできます:

```bash
sudo snap install cloudcompare
```

オプションのインストールについては[公式ページ](https://www.cloudcompare.org/release/index.html#CloudCompare)を
ご確認ください。

## 点群マップのダウンサンプリング
CloudCompare には3 つのサブサンプリング メソッドがあり、ここではサブサンプリングのメソッドを使用していますSpaceが、必要に応じて他のメソッドを使用することもできます。

CloudCompareには3つの[サブサンプリング メソッド](https://www.cloudcompare.org/doc/wiki/index.php/Edit%5CSubsample)あり、
ここではサブサンプリングの`Space` メソッドを使用していますが、必要に応じて他のメソッドを使用することもできます。

1. CloudCompareを開いてポイントクラウドをここにドラッグすると、DBツリーパネルでマップをクリックするだけでポイントクラウドマップを選択できます。
2. `subsample`次に、トップパネルの`subsample`ボタンをクリックします。

<figure markdown>
  ![db-tree-panel](images/select-map.png){ align=center }
  <figcaption>
    CloudCompare
  </figcaption>
</figure>

1. サブサンプル方法を選択してください。tutorial_vehicle用のスペースを使用します。
2. 次に、オプションを選択できます。たとえば、点間の最小スペースを決定する必要があります。(このセクションは注意してください。サブサンプリングはマップサイズ、コンピューターのパフォーマンスなどに依存します。)tutorial_vehicle のマップにはこの値 0.2 を設定します。

<figure markdown>
  ![space-subsampling](images/space-subsampling.png){ align=center width="512" }
  <figcaption>
    点群のサブサンプリング
  </figcaption>
</figure>

- サブサンプリング プロセスが完了したら、
  DBツリーパネルでもポイントクラウドを選択する必要があります。

<figure markdown>
  ![db-tree-panel](images/subsampled-map.png){ align=center width="360" }
  <figcaption>
    ダウンサンプリングされた点群を選択します
  </figcaption>
</figure>

これで、
ダウンサンプリングされた点群を`ctrl + s`で保存する
か、`File`バーから保存ボタンをクリックすることができます。
その後、この点群はAutowareで使用できるようになります。
