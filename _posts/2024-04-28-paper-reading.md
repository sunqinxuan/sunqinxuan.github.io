---
title: '【paper reading】Magnetometer Calibration'
date: 2024-04-28
permalink: /posts/research-journal-2024-04-28-mag-calibration
tags:
  - research journal
---

reading notes.


## Magnetometer Calibration and Alignment to Inertial Sensors

[Dynamic Magnetometer Calibration and Alignment to Inertial Sensors by Kalman Filtering](https://ieeexplore.ieee.org/abstract/document/7866868)

This paper is mainly compared to the gravity-based calibration method, which could be affected by the disturbed acceleration.

> Compared with all previous works, the main contribution
of this brief is a novel state estimation approach collectively
solving the magnetometer intrinsic and cross-sensor
calibrations in a homogeneous magnetic field. 

The magnetomoter measurement model is 

$$
\bf{y}_m=\bf{SC}^b_e\bf{m}^e+\bf{h}+\bf{n}_m
$$

where 

- \\(\bf{m}^e\\) is the local magnetic vector in the earth frame (e-frame), and <u>it is constant and assumed to have unity norm in a homogeneous magnetic field</u>, without loss of generality; <font color=blue>(only the orientation is concerned)</font>
- \\(\bf{n}_m\\) is i.i.d zero-mean Gaussian noise with covariance \\(\sigma_m^2\bf{I}_3\\); 
- The body frame (b-frame) refers to the coordinate frame defined by the gyroscope/accelerometer triads;
- The attitude or orientation matrix \\(\bf{C}^b_e\\) transforms the magnetic field vector from e-frame to b-frame;

> The purpose of the
magnetometer calibration is to determine the matrix \\(\bf{S}\\) and the
vector \\(\bf{h}\\), which <u>collectively encode the magnetometer sensor
triad imperfection, magnetic disturbance, and the misalignment
with respect to b-frame</u>.

![img](http://sunqinxuan.github.io/images/posts-research-journal-2024-04-28-img1.png)

