# ハードウェアデザインリファレンス

このドキュメントはAutoware.Universeソフトウェアでサポートされるセンサーとシステムについて説明および追加情報を提供するために作成されています。

このドキュメントに記載されているすべての機器には利用可能なROS2ドライバーが搭載されており、自動運転車およびロボット工学アプリケーションの現場で1人以上のコミュニティメンバーによってテストされています。

リストされているセンサーとシステムはAutowareコミュニティによって販売、開発、または直接の技術サポートが提供されていません。ただしハードウェアの使用に関するROS2およびAutoware関連の問題は[ここ](https://answers.ros.org/questions/ask/?tags=autoware)にあるコミュニティガイドラインを使用して問い合わせることができます。

このドキュメントは、以下にリストされているセクションで構成されています:

- ADコンピューター

  - ADLINK車載コンピュータ
    - NXP車載コンピュータ
    - Neousys車載コンピュータ
    - Crystal Rugged車載コンピュータ

- LiDAR

  - Velodyne 3D LiDARセンサー
  - Robosense 3D LiDARセンサー
  - HESAI 3D LiDARセンサー
  - Leishen 3D LiDARセンサー
  - Livox 3D LiDARセンサー
  - Ouster 3D LiDARセンサー

- RADAR

  - Smartmicro自動車用Radar
  - Aptiv自動車用Radar
  - Continental工学用Radars

- カメラ

  - FLIRマシンビジョンカメラ
  - Lucidビジョンカメラ
  - Alliedビジョンカメラ
  - Tier IVカメラ
  - Neousysテクノロジーカメラ

- サーマルカメラ

  - FLIRサーマル自動車開発キット

- IMU, AHRS & GNSS/INS

  - NovAtel GNSS/INSセンサー
  - XSens GNSS/INS & IMUセンサー
  - SBG GNSS/INS & IMUセンサー
  - Applanix GNSS/INSセンサー
  - PolyExplore GNSS/INSセンサー
  - Fix Position GNSS/INSセンサー

- ドライブ・バイ・ワイヤ車両供給者
  <!-- cspell: ignore Paravan -->

  - Dataspeed DBW Solutions
  - AStuff Pacmod DBW Solutions
  - Schaeffler-Paravan Space Drive DBW Solutions

- 車両プラットホーム供給者

  - PIX MOVING Autonomous Vehicle Solutions
  - Autonomoustuff AV Solutions
  - NAVYA AV Solutions

- リモート走行

  - FORT ROBOTICS
  - LOGITECH

- すべてのドライバーのリスト

- ADセンサーキット供給者

  - LEO Drive ADセンサーキット
  - TIER IV ADキット
  - RoboSense ADセンサーキット
