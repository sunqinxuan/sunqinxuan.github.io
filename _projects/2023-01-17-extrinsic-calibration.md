---
title: "Online Extrinsic Calibration of RGB and ToF Cameras"
collection: projects
type: "Research Project"
excerpt: '...'
venue: "Visual Navigation"
permalink: /projects/2023-01-17-extrinsic-calibration
date: 2023-01-17
location: "Beijing, China"
---

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img0.png" alt="architecture" />

## Experiment 1

<!--
### 初始标定外参的Canny边缘提取结果

导航左相机图像的Canny边缘提取结果，以绿色显示；

TOF相机图像的Canny边缘提取结果，通过初始标定外参与内参，投影到导航左相机的图像像素坐标系下，以红色显示。

初始外参为离线标定的外参。
-->

### Canny Edge Extraction Results under Initial Calibration Extrinsics

The Canny edge extraction results of the navigation left camera image are displayed in green;

The Canny edge extraction results of the TOF camera image are projected onto the image pixel coordinate system of the navigation left camera through the initial calibration extrinsics and intrinsics and are displayed in red.

The initial extrinsics are calibrated offline.

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img1.png" alt="4_edge_original" style="zoom: 50%;" />

<!--
### 经过外参优化再投影的Canny边缘结果

通过最近邻边缘点关联与导航相机-TOF相机外参优化，使用优化后的外参再投影后的结果。
-->

### Canny Edge Extraction Results After Extrinsics Optimization Reprojection

The results after reprojection using the optimized extrinsics through nearest neighbor edge point association and navigation camera-TOF camera extrinsics optimization.

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img2.png" alt="4_edge_calib" style="zoom: 50%;" />


## Experiment 2

### Canny Edge Extraction Results under Initial Calibration Extrinsics

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img3.png" alt="edges_original" style="zoom:50%;" />

### Canny Edge Extraction Results After Extrinsics Optimization Reprojection

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img4.png" alt="edges_calib" style="zoom:50%;" />

## Experiment 3

### Canny Edge Extraction Results under Initial Calibration Extrinsics

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img5.png" alt="1_edge_org" style="zoom:57%;" />

### Canny Edge Extraction Results After Extrinsics Optimization Reprojection

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img6.png" alt="1_edge_calib" style="zoom:57%;" />

## Experiment 4

### Canny Edge Extraction Results under Initial Calibration Extrinsics

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img7.png" alt="3_edge_org" style="zoom:57%;" />

### Canny Edge Extraction Results After Extrinsics Optimization Reprojection

<img src="https://sunqinxuan.github.io/images/projects-2023-01-17-img8.png" alt="3_edge_calib" style="zoom:57%;" />


## Related Links 

paper:
- [Online Extrinsic Calibration of RGB and ToF Cameras for Extraterrestrial Exploration](https://sunqinxuan.github.io/publication/CCC2023)

code:
- [tof_rgb_calibration](https://github.com/sunqinxuan/tof_rgb_calibration)










