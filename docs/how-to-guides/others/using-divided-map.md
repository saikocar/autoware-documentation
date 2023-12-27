# 分割された点群地図の使用

大規模な点群地図を処理する場合は、分割された点群地図が必要です。その場合、AutowareはROS2トピック経由で地図全体を送信したり、地図全体をメモリにロードしたりできない可能性があります。Autowareは、事前に分割された地図を使用することで、車両の位置に応じて点群地図を動的に読み込みます。

## チュートリアル

[sample-map-rosbag_split](https://docs.google.com/uc?export=download&id=11tLC9T4MS8fnZ9Wo0D8-Ext7hEDl2YJ4)をダウンロードして`$HOME/autoware_map/`下に配置します。

```bash
gdown -O ~/autoware_map/ 'https://docs.google.com/uc?export=download&id=11tLC9T4MS8fnZ9Wo0D8-Ext7hEDl2YJ4'
unzip -d ~/autoware_map/ ~/autoware_map/sample-rosbag_split.zip
```

次に、以下のコマンドでlogging_simulatorを起動し、分割されたマップをロードします。
`map_path`と`pointcloud_map_file`引数を指定する必要があることに注意してください

```bash
source ~/autoware/install/setup.bash
ros2 launch autoware_launch logging_simulator.launch.xml \
  map_path:=$HOME/autoware_map/sample-map-rosbag pointcloud_map_file:=pointcloud_map \
  vehicle_model:=sample_vehicle_split sensor_model:=sample_sensor_kit
```

rosbagを再生してAutowareをシミュレートするには、[rosbag リプレイシミュレーションのチュートリアル](https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/rosbag-replay-simulation/)の手順を参照してください。

## 関連リンク

- 分割地図の具体的なフォーマット定義については[地図コンポーネント設計ページ](https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-architecture/map/)を参照してください。
- [map_loaderのReadme](https://github.com/autowarefoundation/autoware.universe/tree/main/map/map_loader)マップを分割するための具体的な手順として役立つ可能性があります。
- 独自の点群地図を分割するときは[pointcloud_divider](https://github.com/MapIV/pointcloud_divider)を使用できます。これにより、マップを分割し、互換性のあるメタデータを生成できます。
