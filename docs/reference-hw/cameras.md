# カメラ

## **TIER IV車載HDRカメラ**

![camera-tieriv.png](images/camera-tieriv.png)

ROS2ドライバーを搭載しTIER IVによってテストされた[TIER IV車載HDRカメラ](https://sensor.tier4.jp/automotive-camera)は以下のとおりです:

| 対応製品一覧          | MP  | FPS | インターフェース         | HDR           | LFM | トリガー <br> /同期 | 防塵 <br> 防水 | ROS2ドライバー | Autoware <br> テスト済み (Y/N) |
| -------------------------------- | --- | --- | ----------------- | ------------- | --- | ----------------------------- | ----------------------- | ------------ | -------------------------- |
| C1                               | 2.5 | 30  | GMSL2 <br> / USB3 | Y <br>(120dB) | Y   | Y                             | IP69K                   | Y            | Y                          |
| C2                               | 5.4 | 30  | GMSL2 <br> / USB3 | Y <br>(120dB) | Y   | Y                             | IP69K                   | Y            | Y                          |
| C3 <br> (to be released in 2024) | 8.3 | 30  | GMSL2 <br> / TBD  | Y <br>(120dB) | Y   | Y                             | IP69K                   | Y            | Y                          |

ROS2ドライバーへのリンク:  
[https://github.com/tier4/ros2_v4l2_camera](https://github.com/tier4/ros2_v4l2_camera)

製品サポートサイトへのリンク:  
[TIER IV Edge.Auto documentation](https://tier4.github.io/edge-auto-docs/index.html)

製品サイトへのリンク:  
[TIER IV Automotive Camera Solution](https://sensor.tier4.jp/automotive-camera)

## **FLIRマシンビジョンカメラ**

![camera-flir.png](images/camera-flir.png)

ROS2ドライバーを搭載し1人以上のコミュニティメンバーによってテストされたFLIRマシンビジョンカメラのリストは以下のとおりです:

| 対応製品一覧 | MP           | FPS        | インターフェース | HDR | LFM | トリガー <br> /同期 | 防塵 <br> 防水 | ROS2ドライバー | Autowareテスト済み (Y/N) |
| ----------------------- | ------------ | ---------- | --------- | --- | --- | ----------------------------- | ----------------------- | ------------ | --------------------- |
| Blackfly S              | 2.0 <br> 5.0 | 22 <br> 95 | USB-GigE  | N/A | N/A | Y                             | N/A                     | Y            | -                     |
| Grasshopper3            | 2.3 <br> 5.0 | 26 <br> 90 | USB-GigE  | N/A | N/A | Y                             | N/A                     | Y            | -                     |

ROS2ドライバーへのリンク:  
[https://github.com/berndpfrommer/flir_spinnaker_ros2](https://github.com/berndpfrommer/flir_spinnaker_ros2)

企業サイトへのリンク:  
[https://www.flir.eu/iis/machine-vision/](https://www.flir.eu/iis/machine-vision/)

## **Lucidビジョンカメラ**

![camera-lucid_vision.png](images/camera-lucid_vision.png)

ROS2ドライバーを搭載し1人以上のコミュニティメンバーによってテストされたLucidマシンビジョンカメラのリストは以下のとおりです:

| 対応製品一覧 | MP  | FPS  | インターフェース | HDR | LFM | トリガー <br> /同期 | 防塵 <br> 防水 | ROS2ドライバー | Autowareテスト済み (Y/N) |
| ----------------------- | --- | ---- | --------- | --- | --- | ----------------------------- | ----------------------- | ------------ | --------------------- |
| TRITON 054S             | 5.4 | 22   | GigE      | Y   | Y   | Y                             | up to IP67              | Y            | Y                     |
| TRITON 032S             | 3.2 | 35.4 | GigE      | N/A | N/A | Y                             | up to IP67              | Y            | Y                     |

ROS2ドライバーへのリンク:  
[https://gitlab.com/leo-drive/Drivers/arena_camera](https://gitlab.com/leo-drive/Drivers/arena_camera)  
企業サイトへのリンク:  
[https://thinklucid.com/triton-gige-machine-vision/](https://thinklucid.com/triton-gige-machine-vision/)

## **Alliedビジョンカメラ**

![camera-allied_vision.png](images/camera-allied_vision.png)

ROS2ドライバーを搭載し1人以上のコミュニティメンバーによってテストされたAlliedマシンビジョンカメラのリストは以下のとおりです:

| 対応製品一覧 | MP  | FPS  | インターフェース | HDR | LFM | トリガー <br> /同期 | 防塵 <br> 防水 | ROS2ドライバー | Autowareテスト済み (Y/N) |
| ----------------------- | --- | ---- | --------- | --- | --- | ----------------------------- | ----------------------- | ------------ | --------------------- |
| Mako G319               | 3.2 | 37.6 | GigE      | N/A | N/A | Y                             | N/A                     | Y            | -                     |

ROS2ドライバーへのリンク:  
[https://github.com/neil-rti/avt_vimba_camera](https://github.com/neil-rti/avt_vimba_camera)

企業サイトへのリンク:  
[https://www.alliedvision.com/en/products/camera-series/mako-g](https://www.alliedvision.com/en/products/camera-series/mako-g)

## **Neousysテクノロジーカメラ**

![images/camera-neousys.png](images/camera-neousys.png)

ROS2ドライバーを搭載し1人以上のコミュニティメンバーによってテストされたNeousysテクノロジーカメラのリストは以下のとおりです:

| 対応製品一覧 | MP  | FPS | インターフェース                                                                                                                                                                        | センサー形式 | レンズ                                            | ROS2ドライバー | Autowareテスト済み (Y/N) |
| ----------------------- | --- | --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ----------------------------------------------- | ------------ | --------------------- |
| AC-IMX390               | 2.0 | 30  | GMSL2 <br/> (over [PCIe-GL26 Grabber Card](https://www.neousys-tech.com/en/product/product-lines/in-vehicle-computing/vehicle-expansion-card/pcie-gl26-gmsl-frame-grabber-card)) | 1/2.7”        | 5-axis active adjustment with adhesive dispense | Y            | Y                     |

ROS2ドライバーへのリンク:  
[https://github.com/ros-drivers/gscam](https://github.com/ros-drivers/gscam)

企業サイトへのリンク:  
[https://www.neousys-tech.com/en/](https://www.neousys-tech.com/en/)
