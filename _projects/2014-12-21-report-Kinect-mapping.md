---
title: "Kinect-based Localization and Mapping for Service Robot"
collection: projects
type: "Research Project"
excerpt: '...'
venue: "Visual SLAM"
permalink: /projects/2014-12-21-report-Kinect-mapping
date: 2014-12-21
location: "Tianjin, China"
---

<!--家服机器人项目-基于Kinect的室内环境建图-->

## methodology

<a href="http://sunqinxuan.github.io/files/projects-2014-11-18-report-Kinect-mapping.pdf">manuscript (Chinese)</a>。

<!-- ### 帧间扫描匹配

Kinect获取的数据为640*480的RGB图像数据以及各像素点对应的深度数据，基于这样的数据特点，帧间扫描匹配首先利用从RGB图像中提取的特征点及其对应的深度信息来做粗匹配，即完成帧间变换初始值的估计；之后在该初值的基础上，采用ICP（Iterative Closest Point）算法来完成精确匹配。 -->

### Inter-frame Scan Matching

The data obtained by Kinect includes 640x480 RGB images and the corresponding depth data for each pixel. Based on these data characteristics, inter-frame scan matching first uses feature points extracted from the RGB image and their corresponding depth information to perform coarse matching, thereby estimating the initial values for inter-frame transformation. Subsequently, the ICP (Iterative Closest Point) algorithm is used to achieve precise matching based on these initial values.

<!-- ### 全局地图优化

由于逐帧扫描匹配的过程中会有误差的积累，所以需要在此基础上做全局地图优化的工作。全局地图优化是基于家庭服务机器人所在家庭环境中的二维码标识来完成的。二维码标识可以提供机器人在全局环境下较为精确的位姿信息，通过坐标的标定与转换从而能够有效对扫描匹配结果进行校正，完成全局地图的优化。 -->

### Global Map Optimization

Due to the accumulation of errors during frame-by-frame scan matching, global map optimization is necessary. This optimization is based on the QR code markers in the household environment where the service robot operates. QR code markers provide accurate pose information of the robot in the global environment. By calibrating and transforming coordinates, the scan matching results can be effectively corrected, thus optimizing the global map.

<!-- ## 建图测试

实验平台为南开大学机器人与信息自动化研究所的家族服务机器人平台，完成对实验环境的建图，Kinect可以同时采集分辨率为640\\(\times\\)480像素的深度和彩色图像。对同一场景持续测量就可以得到该场景的多幅数据，经过特征点提取、匹配、ICP精确匹配以及全局优化获得环境地图。 -->

## Mapping Test

The experimental platform is the family service robot platform of the Institute of Robotics and Information Automation, Nankai University. The Kinect simultaneously collects depth and color images with a resolution of 640x480 pixels. Multiple data frames of the same scene can be obtained through continuous measurement. After feature point extraction, matching, ICP precise matching, and global optimization, an environment map is generated.

<!-- ### 实验一：全程共342帧点云数据，平均处理速度为2~3fps。 -->
### Experiment 1: A total of 342 frames of point cloud data were collected, with an average processing speed of 2-3 fps.

![img](https://sunqinxuan.github.io/images/project-2014-12-21-img1.PNG)

<!-- ### 实验二：全程共286帧点云数据，平均处理速度为2~3fps。 -->
### Experiment 2: A total of 286 frames of point cloud data were collected, with an average processing speed of 2-3 fps.

![img](https://sunqinxuan.github.io/images/project-2014-12-21-img2.JPG)

<!-- ## 地图匹配精度 -->
## Map Matching Accuracy

<!-- ### 开环精度 -->
### Open Loop Accuracy

<!-- | 单位mm       | 真实位置                     | 扫描匹配位置                 | 误差   |
| ------------ | ---------------------------- | ---------------------------- | ------ |
| mark1        | (-213.349,-130.846,1376.561) | (-215.116,-135.503,1366.638) | 11.103 |
| mark2        | (-4.982,-347.767,1879.321)   | (7.779,-333.014,1888.971)    | 21.763 |
| mark3        | (209.758,-55.875,2283.960)   | (217.240,-38.168,2277.944)   | 20.142 |
| mark4        | (427.871,236.614,2695.830)   | (408.246,243.841,2703.902)   | 22.418 |
| mark5        | (622.753,17.303,3169.669)    | (611.477,26.980,3201.453)    | 35.086 |
| 平均         | 22.1024                      |                              |        |
| 开环匹配精度 | 22.1024                      |                              |        | -->

| Unit: mm     | True Position                 | Scan Matching Position       | Error  |
| ------------ | ------------------------------| ---------------------------- | ------ |
| mark1        | (-213.349, -130.846, 1376.561) | (-215.116, -135.503, 1366.638) | 11.103 |
| mark2        | (-4.982, -347.767, 1879.321)   | (7.779, -333.014, 1888.971)  | 21.763 |
| mark3        | (209.758, -55.875, 2283.960)   | (217.240, -38.168, 2277.944) | 20.142 |
| mark4        | (427.871, 236.614, 2695.830)   | (408.246, 243.841, 2703.902) | 22.418 |
| mark5        | (622.753, 17.303, 3169.669)    | (611.477, 26.980, 3201.453)  | 35.086 |
| Average      | 22.1024                        |                              |        |
| Open Loop Matching Accuracy | 22.1024          |                              |        |

<!-- ### 闭环精度

第243帧闭环，闭环两帧如下图所示（上标出已匹配好的特征点），闭环精度41.85mm。 -->

### Closed Loop Accuracy

Loop closure occurred at frame 243. The two frames forming the loop closure are shown below (with matched feature points indicated), achieving a loop closure accuracy of 41.85 mm.

![img](https://sunqinxuan.github.io/images/project-2014-12-21-img3.jpg)

![img](https://sunqinxuan.github.io/images/project-2014-12-21-img4.jpg)

<!-- ### 栅格地图精度

栅格大小为100\\(\times\\)100mm，下图为二维平面栅格地图原图，其中黑线标识出还原出墙（北面的那排桌子）的位置。 -->

### Grid Map Accuracy

The grid size is 100x100 mm. The figure below shows the original 2D plane grid map, with black lines indicating the position of the restored wall (the row of tables on the north side).

<!-- |              | 栅格地图中 | 实际测量 | 误差    |
| ------------ | ---------- | -------- | ------- |
| 东西墙距离   | 7100.17mm  | 7150mm   | 49.33mm |
| 南墙北桌距离 | 8485.78mm  |          |         | -->

|              | In Grid Map | Actual Measurement | Error    |
| ------------ | ----------- | ------------------ | -------  |
| East-West Wall Distance | 7100.17 mm  | 7150 mm          | 49.33 mm |
| South Wall to North Table Distance | 8485.78 mm  |          |         |

![img](https://sunqinxuan.github.io/images/project-2014-12-21-img5.jpg)

## related links

code:
- [kinect_mapping](https://github.com/sunqinxuan/kinect_mapping)
- [service_robot_mapping](https://github.com/sunqinxuan/service_robot_mapping)