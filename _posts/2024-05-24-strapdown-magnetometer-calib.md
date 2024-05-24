---
title: '磁干扰补偿模型参数估计方法调研'
date: 2024-05-24
permalink: /posts/research-journal-2024-05-24
tags:
  - research journal
---

磁干扰补偿模型参数估计方法调研：

[1] [A Non-linear, Two-step Estimation Algorithm for Calibrating Solid-state Strapdown Magnetometers](https://users.soe.ucsc.edu/~elkaim/Documents/TwoVec.pdf)

[2] [Calibration of Strapdown Magnetometers in the Magnetic Field Domain](https://ascelibrary.org/doi/10.1061/%28ASCE%290893-1321%282006%2919%3A2%2887%29)

[3] [Calibration of a magnetometer in combination with inertial sensors](https://ieeexplore.ieee.org/document/6289882)

[4] [Sensor Fusion and Calibration of Inertial Sensors, Vision, Ultra-Wideband and GPS](https://user.it.uu.se/~thosc112/team/hol2011.pdf)

目前所使用的MIT的基于TL模型的补偿方法，线性最小二乘参数估计方法比较弱，对于噪声和数据异常等情况的鲁棒性较差，而且在建模的时候没有包含飞机姿态参数。另外，由于真值的缺失，使得目前以最小化测量值和真值残差的拟合方法难以顺利进行。

针对上述问题，我对磁强计加惯导的捷联系统整体标定和补偿的方法进行了一些调研。对于干扰磁场模型，大多数文献中都是采用了硬磁干扰加软磁干扰的建模方式，与我们目前所使用的TL模型是一致的。由于没有真值做为参考，因此使用椭圆拟合的原理，对模型参数进行计算，比如文献[1][2]中，使用变量替换的方法，将椭圆方程中的非线性约束转化为线性约束，完成求解后再转换回去。而文献[3][4]的估计模型中，不仅包含了标定参数和补偿参数，还包含传感器每个时刻的姿态，是个高度非线性的问题，通过最大似然估计进行求解。但是这样的求解方法需要给一个离最优解相对较近的初值，不然可能不收敛，因此使用椭圆约束和一些其他约束条件，先一步进行初值的估计。关于这一部分，我还没有太理清。但我觉得这样的方法对于我想要将飞行姿态考虑进行的想法还是比较有启发的，所以准备进一步研究一下。

不考虑任何误差与干扰的情况下，在均匀磁场转运载体时，测量磁场矢量的模长不受姿态影响，在三维空间中其轨迹落在球面上。当受到误差和干扰的影响时，理论上磁场测量矢量的轨迹形成一个椭球面。

> For stand-alone magnetometers the mapping of the ellipsoid
of data to a sphere gives a complete magnetometer calibration.
However, combining the magnetometer with inertial sensors
requires the sensor axes to be aligned, i.e. <u>the sphere needs to
be rotated such that the magnetometer sensor axes are aligned
with the inertial sensor axes.</u>

这一段还不是很理解。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="http://sunqinxuan.github.io/images/posts-research-journal-2024-05-24-img0.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图片来自《地磁导航理论与实践》（张晓明）</div>
</center>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="http://sunqinxuan.github.io/images/posts-research-journal-2024-05-24-img1.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图片来自"Calibration of a magnetometer in combination with inertial sensors"</div>
</center>

磁强计-惯导捷联系统的测量模型为

$$
\boldsymbol{y}_{m,k}^b=C_{sc}C_{no}
(C_{si}R_{im}\boldsymbol{m}_k^b+\boldsymbol{o}_{hi})
+\boldsymbol{o}_{zb}+\boldsymbol{e}_{m,k}^b
$$

其中

- \\(\boldsymbol{y}_{m,k}^b\\)为磁场测量数据；

- \\(\boldsymbol{m}_k^b\\)是在body frame中表示的地磁矢量，其与navigation frame中的地磁矢量之间的关系为

$$
\boldsymbol{m}_k^b=R_k^{bn}\boldsymbol{m}^n
$$

- \\(C_{sc}\\)是磁强计三轴灵敏度矩阵；

- \\(C_{no}\\)是磁强计三轴非正交矩阵；

- \\(\boldsymbol{o}_{zb}\\)是磁强计三轴零偏；

- \\(\boldsymbol{e}_{m,k}^b\\)是磁强计的测量噪声；

- \\(C_{si}\\)为载体干扰磁场中的感应磁场矩阵（soft-iron effect）；

- \\(\boldsymbol{o}_{hi}\\)为载体干扰磁场中的固定磁场向量（hard-iron effects）。












































