---
title: "Orientation Estimation for Triaxial Magnetometer in Non-Uniform Magnetic Field Environments"
collection: projects
type: "Research Project"
excerpt: '...'
venue: "Geomagnetic Navigation"
permalink: /projects/2024-05-13-mag-alignment
date: 2024-05-13
location: "Beijing, China"
---

<!--非均匀磁场环境下矢量磁测量系统定姿方案 
Orientation Estimation for Triaxial Magnetometer in Non-Uniform Magnetic Field Environments.-->

## Background

<!--
在[基于Tolles-Lawson模型的航磁补偿系统](https://sunqinxuan.github.io/projects/2024-05-06-mag-compensation)中，提到通过在舱外安装矢量磁强计来同步获取地磁场信息，完成矢量干扰磁场的标定。另外，在基于深度学习方法的磁补偿方案中，真值信息的获取同样也是一个亟待解决的问题。

本方案提出非均匀磁场环境中的矢量磁强计定姿方案，使用高精度转台以及水平仪等设备，完成矢量磁强计的坐标系配准。-->

In the Tolles-Lawson model-based aeromagnetic compensation system, it is mentioned that external vector magnetometers are installed outside the cabin to synchronously obtain geomagnetic field information, thereby completing the calibration of vector disturbance magnetic fields. Additionally, in magnetic compensation solutions based on deep learning methods, acquiring true value information is also an urgent problem to be solved.

This proposal presents an orientation estimation scheme for triaxial magnetometers in non-uniform magnetic field environments. It utilizes high-precision turntables and devices like spirit levels to achieve coordinate alignment for vector magnetometers.


## Orientation Estimation for Triaxial Magnetometers

<!--
假设两套矢量磁测量系统所在磁场的大小与方向均不同，但都是恒定磁场，不会随时间以及传感器转动而发生变化。

在其中一套测量系统下加装高精度旋转机构，记为系统1，另一套记为系统2，并对两套系统同时进行水平调平，不失一般性，使\\(Z\\)轴平行于重力方向，如下图所示。-->

Assuming two triaxial magnetometers are in magnetic fields of different magnitudes and directions, both constant and unaffected by time or sensor rotation.

Sensor 1 (with high-precision rotating mechanism) and Sensor 2 are both horizontally leveled simultaneously for convenience, aligning the \\(Z\\) axis parallel to the direction of gravity, as illustrated in the diagram below.

![img](https://sunqinxuan.github.io/images/projects-2024-05-13-img2.png)

<!--系统2对矢量磁场的测量结果为-->
The measurement results of Sensor 2 for the magnetic field are

$$
\vec{B}_r=
\begin{bmatrix}
B_{rx} \\
B_{ry} \\
B_{rz} \\
\end{bmatrix}
$$

<!--其中，符号\\(r\\)表示系统2的附体坐标系。

固定系统1下面的高精度转台，获得矢量磁场测量结果为-->

where symbol \\(r\\) denotes the body coordinate system of Sensor 2.

Fixing the high-precision turntable below Sensor 1, the measurement results for the magnetic field are obtained as

$$
\vec{B}_c=
\begin{bmatrix}
B_{cx} \\
B_{cy} \\
B_{cz} \\
\end{bmatrix}
$$

<!--类似的，符号\\(c\\)表示系统1的附体坐标系。

将\\(\vec{B}_c\\)变换到坐标系\\(r\\)，可得-->

Similarly, symbol \\(c\\) represents the body coordinate system of Sensor 1.

Transforming \\(\vec{B}_c\\) to the coordinate system \\(r\\), we obtain

$$
{}^r\vec{B}_c=R_{rc}(\theta)\vec{B}_c=
\begin{bmatrix}
\cos\theta & -\sin\theta & 0 \\
\sin\theta & \cos\theta & 0 \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
B_{cx} \\
B_{cy} \\
B_{cz} \\
\end{bmatrix}
$$

<!--由此可得在坐标系\\(r\\)下表示的矢量磁场差为-->
Thus, the difference of magnetic measurements represented in the coordinate system \\(r\\) is

$$
\Delta\vec{B}=\vec{B}_r-{}^r\vec{B}_c
=\vec{B}_r-R_{rc}(\theta)\vec{B}_c
\tag{1}
$$

<!--其中，磁场的矢量差\\(\Delta\vec{B}\\)以及两个测量系统的角度差\\(\theta\\)为待求解变量。因此可以看出，式(1)提供了该参数辨识问题的一个约束条件。

通过高精度转台，将系统1旋转角度\\(\alpha_1\\)后，此时系统1的坐标系记为\\(c1\\)，可以得到第二个约束条件。-->

where the difference of magnetic measurements \\(\Delta\vec{B}\\) and the angle difference \\(\theta\\) between the two magnetometers are the variables to be solved. Thus, Equation (1) provides a constraint for this parameter identification problem.

Using the high-precision turntable, after rotating Sensor 1 by an angle \\(\alpha_1\\), its coordinate system at this time is denoted as \\(c1\\), providing the second constraint condition.

$$
\Delta\vec{B}=\vec{B}_r-{}^r\vec{B}_{c1}
=\vec{B}_r-R_{rc}(\theta+\alpha_1)\vec{B}_{c1}
\tag{2}
$$

<!--类似地，将系统1旋转\\(N\\)个角度后，并把所有的约束条件写在一起，可以得到：-->
Similarly, after rotating Sensor 1 by \\(N\\) angles and combining all the constraints together, we obtain

$$
\begin{aligned}
&\Delta\vec{B}=\vec{B}_r-{}^r\vec{B}_c
=\vec{B}_r-R_{rc}(\theta)\vec{B}_c \\
&\Delta\vec{B}=\vec{B}_r-{}^r\vec{B}_{c1}
=\vec{B}_r-R_{rc}(\theta+\alpha_1)\vec{B}_{c1}\\
&\Delta\vec{B}=\vec{B}_r-{}^r\vec{B}_{c2}
=\vec{B}_r-R_{rc}(\theta+\alpha_2)\vec{B}_{c2}\\
&\quad\quad\quad \vdots \\
&\Delta\vec{B}=\vec{B}_r-{}^r\vec{B}_{cN}
=\vec{B}_r-R_{rc}(\theta+\alpha_N)\vec{B}_{cN}\\
\end{aligned}
$$

<!--至此，可以通过非线性最小二乘等辨识方法，求解未知参数\\(\Delta\vec{B}\\)和\\(\theta\\)，解决非均匀磁场中两个矢量磁测量系统的定姿问题。-->
Thus, using nonlinear least squares or similar identification methods, we can solve for the unknown parameters \\(\Delta\vec{B}\\) and \\(\theta\\), addressing the orientation problem of two triaxial magnetometers in a non-uniform magnetic field.

![img](https://sunqinxuan.github.io/images/projects-2024-05-13-img1.jpg)