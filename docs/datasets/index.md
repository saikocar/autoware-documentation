# データセット

Autowareパートナーは、テストと開発用のデータセットを提供します。これらのデータセットはここからダウンロードできます。

## Bus-ODD (オペレーショナルデザインドメイン)データセット

### Leo Drive - ISUZUセンサーデータ

このデータセットには、Bus ODDプロジェクトで使用されるいすゞバスのデータが含まれています。

データには次のセンサーからのデータが含まれます:

- 1 x VLP16
- 2 x VLP32C
- 1 x Applanix POS LV 120 GNSS/INS
- 3 x Lucid Vision Triton 5.4MPカメラ (左、右、前)
- 車両状況レポート

また、センサー間の静的変換のための`/tf`トピックも含まれています。

#### 必要なメッセージの種類

GNSSデータは、`sensor_msgs/msg/NavSatFix`メッセージタイプで利用できます。

ただしApplanixの生メッセージは、`applanix_msgs/msg/NavigationPerformanceGsof50`および`applanix_msgs/msg/NavigationSolutionGsof49`メッセージタイプにも含まれます。
これらのメッセージを再生できるようにするには、`applanix_msgs`パッケージをビルドしてソースする必要があります。

```bash
# Create a workspace and clone the repository
mkdir -p ~/applanix_ws/src && cd "$_"
git clone https://github.com/autowarefoundation/applanix.git
cd ..

# Build the workspace
colcon build --symlink-install --packages-select applanix_msgs

# Source the workspace
source ~/applanix_ws/install/setup.bash

# Now you can play back the messages
```

また、必ずAutoware Universeワークスペースもソースしてください。

#### ダウンロード手順

```console
# Install awscli
$ sudo apt update && sudo apt install awscli -y

# This will download the entire dataset to the current directory.
# (About 10.9GB of data)
$ aws s3 sync s3://autoware-files/collected_data/2022-08-22_leo_drive_isuzu_bags/ ./2022-08-22_leo_drive_isuzu_bags  --no-sign-request

# Optionally,
# If you instead want to download a single bag file, you can get a list of the available files with following:
$ aws s3 ls s3://autoware-files/collected_data/2022-08-22_leo_drive_isuzu_bags/ --no-sign-request
   PRE all-sensors-bag1_compressed/
   PRE all-sensors-bag2_compressed/
   PRE all-sensors-bag3_compressed/
   PRE all-sensors-bag4_compressed/
   PRE all-sensors-bag5_compressed/
   PRE all-sensors-bag6_compressed/
   PRE driving_20_kmh_2022_06_10-16_01_55_compressed/
   PRE driving_30_kmh_2022_06_10-15_47_42_compressed/

# Then you can download a single bag file with the following:
aws s3 sync s3://autoware-files/collected_data/2022-08-22_leo_drive_isuzu_bags/all-sensors-bag1_compressed/ ./all-sensors-bag1_compressed  --no-sign-request
```

### AutoCore.ai - ROS2バグファイルとpcap

このデータセットにはOuster OS1-64 Lidarのpcapファイルとros2バグファイルが含まれています。
pcapファイルとros2バグファイルは、時間にわずかな違いはありますが、同時に記録されます。

[ここをクリックしてダウンロードしてください (~553MB))](https://autoware-files.s3.us-west-2.amazonaws.com/collected_data/2022-04-14_autocore-lidar-bag-pcap/Lidar_Data_220414_bag_pcap.zip)

[問題の参考](https://github.com/autowarefoundation/autoware.universe/issues/562#issuecomment-1102662448)
