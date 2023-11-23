# IMU, AHRS & GNSS/INS

## **NovAtel GNSS/INSセンサー**

![images/gnss-novatel.png](images/gnss-novatel.png)

ROS2ドライバーがあり1人以上のコミュニティメンバーにテストされたNovAtel GNSS/INSセンサーの一覧は以下のとおりです:

| 対応製品一覧 | INS Rate | Roll, Pitch, Yaw 精度                  | GNSS                                    | ROS2ドライバー  | Autowareテスト済み (Y/N) |
| ----------------------- | -------- | -------------------------------------- | --------------------------------------- | ------------- | --------------------- |
| PwrPak7D-E2             | 200 Hz   | R (0.013°)<br>P (0.013°)<br>Y (0.070°) | 20 Hz<br>L1 / L2 / L5<br> 555 Channels  | Y             | -                     |
| Span CPT7               | 200 Hz   | R (0.01°) <br>P (0.01°) <br>Y (0.03°)  | 20 Hz <br>L1 / L2 / L5 <br>555 Channels | Y             | -                     |

ROS2ドライバーへのリンク:  
[https://github.com/swri-robotics/novatel_gps_driver/tree/dashing-devel](https://github.com/swri-robotics/novatel_gps_driver/tree/dashing-devel)

企業サイトへのリンク:  
[https://hexagonpositioning.com/](https://hexagonpositioning.com/)

## **XSens GNSS/INS & IMUセンサー**

![images/gnss-novatel.png](images/gnss-xsens.png)

ROS2ドライバーがあり1人以上のコミュニティメンバーにテストされたXSens GNSS/INSセンサーの一覧は以下のとおりです:

| 対応製品一覧 | INS/IMU Rate | Roll, Pitch, Yaw 精度            | GNSS                             | ROS2ドライバー  | Autowareテスト済み (Y/N) |
| ----------------------- | ------------ | -------------------------------- | -------------------------------- | ------------- | --------------------- |
| MTi-680G                | 2 kHz        | R (0.2°)<br>P (0.2°)<br>Y (0.5°) | 5 Hz<br>L1 / L2 <br>184 Channels | Y             | -                     |
| MTi-300 AHRS            | 2 kHz        | R (0.2°)<br>P (0.2°)<br>Y (1°)   | Not Applicable                   | Y             | -                     |

ROS2ドライバーへのリンク:  
[http://wiki.ros.org/xsens_mti_driver](http://wiki.ros.org/xsens_mti_driver)

企業サイトへのリンク:  
[https://www.xsens.com/](https://www.xsens.com/)

## **SBG GNSS/INS & IMUセンサー**

![images/gnss-sbg.png](images/gnss-sbg.png)

ROS2ドライバーがあり1人以上のコミュニティメンバーにテストされたSBG GNSS/INSセンサーの一覧は以下のとおりです:

| 対応製品一覧 | INS/IMU Rate        | Roll, Pitch, Yaw 精度             | GNSS                            | ROS2ドライバー  | Autowareテスト済み (Y/N) |
| ----------------------- | ------------------- | --------------------------------- | ------------------------------- | ------------- | --------------------- |
| Ellipse-D               | 200 Hz, 1 kHz (IMU) | R (0.1°)<br>P (0.1°)<br>Y (0.05°) | 5 Hz<br>L1 / L2<br>184 Channels | Y             | Y                     |
| Ellipse-A (AHRS)        | 200 Hz, 1 kHz (IMU) | R (0.1°)<br>P (0.1°)<br>Y (0.8°)  | Not Applicable                  | Y             | -                     |

ROS2ドライバーへのリンク:  
[https://github.com/SBG-Systems/sbg_ros2](https://github.com/SBG-Systems/sbg_ros2)

企業サイトへのリンク:  
[https://www.sbg-systems.com/products/ellipse-series/](https://www.sbg-systems.com/products/ellipse-series/)

## **Applanix GNSS/INS Sensors**

  <!-- cspell: ignore  POSLV  POLYNAV -->

![images/gnss-applanix.png](images/gnss-applanix.png)

ROS2ドライバーがあり1人以上のコミュニティメンバーにテストされたSBG GNSS/INS(原文ママ)センサーの一覧は以下のとおりです:

| 対応製品一覧 | INS/IMU Rate | Roll, Pitch, Yaw 精度               | GNSS                         | ROS2ドライバー  | Autowareテスト済み (Y/N) |
| ----------------------- | ------------ | ----------------------------------- | ---------------------------- | ------------- | --------------------- |
| POSLVX                  | 200 Hz       | R (0.03°)<br>P (0.03°)<br>Y (0.09°) | L1 / L2 / L5<br>336 Channels | Y             | Y                     |
| POSLV220                | 200 Hz       | R (0.02°)<br>P (0.02°)<br>Y (0.05°) | L1 / L2 / L5<br>336 Channels | Y             | Y                     |

ROS2ドライバーへのリンク:  
[http://wiki.ros.org/applanix_driver](http://wiki.ros.org/applanix_driver)

企業サイトへのリンク:  
[https://www.applanix.com/products/poslv.htm](https://www.applanix.com/products/poslv.htm)

## **PolyExplore GNSS/INSセンサー**

![images/gnss-polyexplore.png](images/gnss-polyexplore.png)

ROS2ドライバーがあり1人以上のコミュニティメンバーにテストされたPolyExplore GNSS/INSセンサーの一覧は以下のとおりです:

| 対応製品一覧 | INS/IMU Rate | Roll, Pitch, Yaw 精度                 | GNSS                    | ROS2ドライバー  | Autowareテスト済み (Y/N) |
| ----------------------- | ------------ | ------------------------------------- | ----------------------- | ------------- | --------------------- |
| POLYNAV 2000P           | 100 Hz       | R (0.01°)<br>P (0.01°)<br>Y (0.1°)    | L1 / L2<br>240 Channels | Y             | -                     |
| POLYNAV 2000S           | 100 Hz       | R (0.015°)<br>P (0.015°)<br>Y (0.08°) | L1 / L2<br>40 Channels  | Y             | -                     |

ROS2ドライバーへのリンク:  
[https://github.com/polyexplore/ROS2_Driver](https://github.com/polyexplore/ROS2_Driver)

企業サイトへのリンク:  
[https://www.polyexplore.com/](https://www.polyexplore.com/)

## **Fix Position GNSS/INSセンサー**

![images/gnss-fixposition.png](images/gnss-fixposition.png)

| 対応製品一覧 | INS/IMU Rate | Roll, Pitch, Yaw 精度 | GNSS            | ROS2ドライバー  | Autowareテスト済み (Y/N) |
| ----------------------- | ------------ | --------------------- | --------------- | ------------- | --------------------- |
| Vision-RTK 2            | 200Hz        | -                     | 5 Hz<br>L1 / L2 | Y             | -                     |

ROS2ドライバーへのリンク:   
[https://github.com/fixposition/fixposition_driver](https://github.com/fixposition/fixposition_driver)

企業サイトへのリンク:  
[https://www.fixposition.com/](https://www.fixposition.com/)
