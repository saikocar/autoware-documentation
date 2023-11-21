# インストール

このドキュメントでは[AWF Autoware Core/Universe](https://github.com/autowarefoundation/autoware)と`scenario_simulator_v2`の構築方法に関する段階的な説明をします。

## 前提条件

1. [Autowareがビルド済みでインストールされていること](../../../installation/)

## ビルドの方法

1. Autowareのワークスペースに移動します:

   ```bash
   cd autoware
   ```

2. シミュレータの依存関係をインポートします:

   ```bash
   vcs import src < simulator.repos
   ```

3. 依存するROSパッケージをインポートします:

   ```bash
   source /opt/ros/humble/setup.bash
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

5. ワークスペースをビルドします:

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```
