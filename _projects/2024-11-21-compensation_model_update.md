---
title: 'Online Update of Compensation Model'
collection: projects
type: "Research Project"
excerpt: '...'
venue: "Geomagnetic Navigation"
permalink: /projects/2024-11-21-compensation
date: 2024-11-21
location: "Beijing, China"
---

<!--磁补偿模型的在线自演进更新方法-->

<!--
基于椭球误差模型的磁补偿方法，在补偿模型标定时，需要基于载体处于均匀磁场环境的假设，因此，不可避免引入磁场梯度带来的误差。
由于在标定数据采集过程中，会同步采集舱外磁场数据作为磁补偿模型训练的真值，另外，在地磁导航的过程中，也会实时获取磁图数据来进行磁场匹配，因此，可以利用这些信息，来进行在线磁补偿模型的更新。
-->
The magnetometer compensation method based on the ellipsoidal error model requires the assumption that the carrier operates in a uniform magnetic field environment during the calibration of the compensation model. However, this inevitably introduces errors caused by magnetic field gradients.  

During the calibration data collection process, external magnetic field data is simultaneously collected as ground truth for training the compensation model. Additionally, in geomagnetic navigation, magnetic map data is acquired in real-time for magnetic field matching. Therefore, this information can be leveraged to perform online updates of the magnetic compensation model.

## Sensor Model

The measurement model is given as described [here](https://sunqinxuan.github.io/projects/2024-07-09-compensation).

$$
\boldsymbol{y}_{k}^m=D\boldsymbol{m}_k^m+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

The measurements of the magnetometer outside the cabin are denoted by the superscript \\(m'\\).

$$
\boldsymbol{y}_{k}^m=DR^{mm'}\boldsymbol{m}_k^{m'}+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

let 

$$
C=DR^{mm'}
$$

then

$$
\boldsymbol{y}_{k}^m=C\boldsymbol{m}_k^{m'}+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

## Sensor Calibration with Known Magnetic Field

Instead of rotating the platform in a homogeneous magnetic field, the calibration parameters can also be ensured by changing the external magnetic field, e.g., by moving the sensor in an inhomogeneous but known magnetic field.

Define the compensation model parameters

$$
\theta=
\begin{bmatrix}
o \\
{\rm vec}(H)
\end{bmatrix}
$$

The sensor model can be rewritten as (drop the superscript for convenience)

$$
\boldsymbol{y}_{k}=h(\boldsymbol{m}_k)\theta+\boldsymbol{e}_{k}
$$

where 

$$
h(\boldsymbol{m}_k)=
\begin{bmatrix}
I_{3\times3} &
x_{mk}I_{3\times3} &
y_{mk}I_{3\times3} &
z_{mk}I_{3\times3}
\end{bmatrix}
$$




![img](http://sunqinxuan.github.io/images/projects-2024-11-21-img1.jpg)

![img](http://sunqinxuan.github.io/images/projects-2024-11-21-img2.jpg)






## Related Links 

code:
- [magnetic_compensation](https://github.com/sunqinxuan/magnetic_compensation)


