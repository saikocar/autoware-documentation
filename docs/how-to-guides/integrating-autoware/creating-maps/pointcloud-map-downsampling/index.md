点群マップのダウンサンプリング
概要
作成した点群マップが密すぎるか大きすぎる場合 (つまり、300 MB を超える場合)、計算効率とメモリ効率を向上させるためにダウンサンプリングすることができます。また、部分読み込みを伴う動的マップ読み込みの使用を検討することもできます。詳細については、map_loader パッケージを確認してください。

tutorial_vehicle の実装ではマップ全体を使用するため、CloudCompare を使用してダウンサンプリングします。

CloudCompareのインストール
スナップでインストールできます。

sudo snap install cloudcompare
オプションの取り付けについては公式ページをご確認ください 。

点群マップのダウンサンプリング
CloudCompare には3 つのサブサンプリング メソッドがあり、ここではサブサンプリングのメソッドを使用していますSpaceが、必要に応じて他のメソッドを使用することもできます。

CloudCompare を開いてポイントクラウドをここにドラッグすると、DB ツリー パネルでマップをクリックするだけでポイントクラウド マップを選択できます。
subsample次に、トップパネルの ボタンをクリックします。
![db-tree-panel](images/select-map.png){ align=center } CloudCompare
サブサンプル方法を選択してください。tutorial_vehicle 用のスペースが使用されます。
次に、オプションを選択できます。たとえば、点間の最小スペースを決定する必要があります。(このセクションでは注意してください。サブサンプリングはマップ サイズ、コンピューターのパフォーマンスなどに依存します。)tutorial_vehicle のマップにはこの値 0.2 を設定します。
![space-subsampling](images/space-subsampling.png){ align=center width="512" } 点群のサブサンプリング
サブサンプリング プロセスが完了したら、[DB ツリー] パネルでもポイントクラウドを選択する必要があります。
![db-tree-panel](images/subsampled-map.png){ align=center width="360" } ダウンサンプリングされた点群を選択します
これで、ダウンサンプリングされた点群を保存するctrl + s か、バーから保存ボタンをクリックすることができますFile。その後、この点群は Autoware で使用できるようになります。
# Pointcloud map downsampling

## Overview

When your created point cloud map is either too dense or too large (i.e., exceeding 300 MB),
you may want to downsample it for improved computational and memory efficiency.
Also, you can consider using dynamic map loading with partial loading,
please check [map_loader package](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_loader) for more information.

At tutorial_vehicle implementation we will use the whole map,
so we will downsample it with using [CloudCompare](https://www.cloudcompare.org/main.html).

## Installing CloudCompare

You can install it by snap:

```bash
sudo snap install cloudcompare
```

Please check the [official page](https://www.cloudcompare.org/release/index.html#CloudCompare)
for installing options.

## Downsampling a pointcloud map

There are three [subsampling methods on CloudCompare](https://www.cloudcompare.org/doc/wiki/index.php/Edit%5CSubsample),
we are using `Space` method for subsampling, but you can use other methods if you want.

1. Please open CloudCompare and drag your pointcloud to here, then you can select your pointcloud map by just clicking on the map at the DB tree panel.
2. Then you can click `subsample` button on the top panel.

<figure markdown>
  ![db-tree-panel](images/select-map.png){ align=center }
  <figcaption>
    CloudCompare
  </figcaption>
</figure>

1. Please select on your subsample method, we will use space for tutorial_vehicle.
2. Then you can select options. For example, we need to determine minimum space between points. (Please be careful in this section, subsampling is depending on your map size, computer performance, etc.) We will set this value 0.2 for tutorial_vehicle's map.

<figure markdown>
  ![space-subsampling](images/space-subsampling.png){ align=center width="512" }
  <figcaption>
    Pointcloud subsampling
  </figcaption>
</figure>

- After the subsampling process is finished,
  you should select pointcloud on the DB Tree panel as well.

<figure markdown>
  ![db-tree-panel](images/subsampled-map.png){ align=center width="360" }
  <figcaption>
    Select your downsampled pointcloud
  </figcaption>
</figure>

Now,
you can save your downsampled pointcloud with `ctrl + s`
or you can click save button from `File` bar.
Then, this pointcloud can be used by autoware.
