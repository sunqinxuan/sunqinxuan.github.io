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

## Magnetometer measurement model

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

---

Rewriting Eqn.(10) yields

$$
\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o})=R\boldsymbol{m}^{m'}_k
$$

Let 

$$
\begin{aligned}
& p_k'\triangleq\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o}) \\
& p_k\triangleq \boldsymbol{m}^{m'}_k
\end{aligned}
$$

Then, the initial value estimation method mentioned above can be applied here.
Given an initial value, the ceres optimization problem can be constructed to solve Eqn.(10).


## Magnetometer measurement model

<!--磁强计-惯导捷联系统的测量模型为-->
The measurement model of the magnetometer-inertial navigation system is

$$
\boldsymbol{y}_{k}^m=C_{sc}C_{no}
(C_{si}\boldsymbol{m}_k^m+\boldsymbol{o}_{hi})
+\boldsymbol{o}_{zb}+\boldsymbol{e}_{k}^m
$$

<!--
其中

- \\(\boldsymbol{y}_{k}^m\\)为矢量磁场测量数据；

- \\(\boldsymbol{m}_k^m\\)是在magnetometer frame中表示的地磁矢量；

- \\(C_{sc}\\)是磁强计三轴灵敏度矩阵；

- \\(C_{no}\\)是磁强计三轴非正交矩阵；

- \\(\boldsymbol{o}_{zb}\\)是磁强计三轴零偏；

- \\(\boldsymbol{e}_{k}^m\\)是磁强计的测量噪声；

- \\(C_{si}\\)为载体干扰磁场中的感应磁场矩阵（soft-iron effect）；

- \\(\boldsymbol{o}_{hi}\\)为载体干扰磁场中的固定磁场向量（hard-iron effects）；
-->

where

- \\(\boldsymbol{y}_{k}^m\\) is the magnetic field measurement data; 

- \\(\boldsymbol{m}_k^m\\) is the geomagnetic vector represented in the magnetometer frame;

- \\(C_{sc}\\) is the magnetometer triaxial sensitivity matrix;

- \\(C_{no}\\) is the magnetometer triaxial non-orthogonal matrix;

- \\(\boldsymbol{o}_{zb}\\) is the magnetometer triaxial zero bias;

- \\(\boldsymbol{e}_{k}^m\\) is the measurement noise of the magnetometer;

- \\(C_{si}\\) is the induced magnetic field matrix in the carrier interference field (soft-iron effect);

- \\(\boldsymbol{o}_{hi}\\) is the fixed magnetic field vector in the carrier interference field (hard-iron effects).

<!--进一步整理后，得到最终的测量模型：-->
After further organization, the final measurement model is obtained as follows.

$$
\boldsymbol{y}_{k}^m=D\boldsymbol{m}_k^m+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

where

$$
D=C_{sc}C_{no}C_{si}
$$

$$
\boldsymbol{o}=C_{sc}C_{no}\boldsymbol{o}_{hi}+\boldsymbol{o}_{zb}
$$

<!--
上述模型在目前的研究中是比较通用的，该模型能够成立的一些假设条件：

- 干扰磁场须来自于与传感器刚性固连的设备或者载体平台；

- 标定数据采集时，外部磁场（地磁场）是均匀不变的。

在本课题中，以上两个假设均可以视作已满足。
-->

The above model is relatively common in current research. Some assumptions for the model to hold include:

- The interference magnetic field must come from equipment or carrier platforms rigidly attached to the sensor.

- During calibration data collection, the external magnetic field (geomagnetic field) is uniform and constant.

In this study, both assumptions can be considered as satisfied.

## Quadrics

<!--二次曲面包含不同的形状类型，有圆球、椭球、抛物面等。一般形式的二次曲面方程可以表示为-->
Quadratic surfaces include different shapes such as spheres, ellipsoids, and paraboloids. The general form of the quadratic surface equation can be expressed as

$$
S:
ax^2+by^2+cz^2+2fyz+2gxz+2hxy+2px+2qy+2rz+d=0.
$$

<!--其矩阵形式为-->
The matrix form of the above equation is represented by 

$$
S:
\boldsymbol{x}^T\boldsymbol{M}\boldsymbol{x}+
\boldsymbol{x}^T\boldsymbol{n}+d=0
$$

where

$$
\boldsymbol{x}=
\begin{bmatrix}
x\\ y\\ z
\end{bmatrix}
$$

$$
\boldsymbol{M}=
\begin{bmatrix}
a & h & g \\
h & b & f \\
g & f & c \\
\end{bmatrix}
$$

$$
\boldsymbol{n}=
\begin{bmatrix}
p\\ q\\ r
\end{bmatrix}
$$

## Ellipsoid fit

<!--首先，令\\(\mathcal{F}\\)表示载体所在区域的均匀地磁场强度，即-->
First, let \\(\mathcal{F}\\) represent the uniform geomagnetic field strength in the area where the carrier is located, that is,

$$
\|{m}^n\|_2=\mathcal{F}
$$

<!--则有以下等式成立-->
Then the following equation holds.

$$
\|{m}^n\|_2^2-\mathcal{F}^2=\|R_k^{nm}{m}_k^m\|_2^2-\mathcal{F}^2=\|{m}_k^m\|_2^2-\mathcal{F}^2=0
$$

<!--展开可得椭球约束方程：-->
Expanding this, we obtain the ellipsoid constraint equation as follows.

$$
\begin{aligned}
0&=\|D^{-1}({y}_{k}^m-{o}-{e}_{k}^m)\|_2^2-\mathcal{F}^2 \\
&\approx{y_{k}^m}^TA{y}_{k}^m+2{b}^T{y}_{k}^m+c
\end{aligned}
$$

where

$$
A=D^{-T}D^{-1} \tag{1}
$$

$$
{b}^T=-{o}^TD^{-T}D^{-1} \tag{2}
$$

$$
c={o}^TD^{-T}D^{-1}{o}-\mathcal{F}^2 \tag{3}
$$

<!--椭球拟合可以参考文献[^6]所提出的方法，及其对应的matlab实现版本[^7]。通过椭球拟合方法，可以得到椭球系数的估计值\\(\hat{A}_s,\hat{b}_s,\hat{c}_s\\)。 -->
Ellipsoid fitting can refer to the method proposed in reference [^6] and its corresponding MATLAB implementation [^7]. Using the ellipsoid fitting method, estimates \\(\hat{A}_s,\hat{b}_s,\hat{c}_s\\) of the ellipsoid coefficients can be obtained.

<!--从椭球方程可以看出，对于任意\\(\alpha\in\mathbb{R}\\)，\\(\alpha\hat{A}_s,\alpha\hat{b}_s,\alpha\hat{c}_s\\)同样可以满足该椭圆方程。 -->
From the ellipsoid equation, it can be observed that for any \\(\alpha\in\mathbb{R}\\)，\\(\alpha\hat{A}_s,\alpha\hat{b}_s,\alpha\hat{c}_s\\) also satisfy the ellipsoid equation. 

$$
\begin{aligned}
\mathcal{F}^2&={o}^TD^{-T}D^{-1}{o}-c \\
&=b^TA^{-1}b-c \\
&=\alpha(\hat{b}_s^T\hat{A}_s^{-1}\hat{b}_s-\hat{c}_s)
\end{aligned}
$$

<!--根据式(3)提供的约束，可以确定变量\\(\alpha\\)的值。 -->
According to the constraint provided by equation (3), the value of the variable \\(\alpha\\) can be determined.

$$
\alpha=(\hat{b}_s^T\hat{A}_s^{-1}\hat{b}_s-\hat{c}_s)^{-1}\mathcal{F}^2
$$

<!--接下来，根据式(1)-(3)，对模型参数的估计结果\\(\hat{D},\hat{o}\\)进行反解。 -->
Next, based on equations (1)-(3), the estimated model parameters \\(\hat{D},\hat{o}\\) are solved.

$$
\hat{D}^{-T}\hat{D}^{-1}=\alpha\hat{A}_s \tag{4}
$$

$$
\hat{o}=-\hat{A}_s^{-1}\hat{b}_s
$$

<!--根据式(4)可以看出，\\(\hat{D}\\)没有唯一解，因为对于任意满足\\(V^TV=I_3\\)的矩阵\\(V\\)，\\(\hat{D}V\\)都可以使式(4)成立。

通过对式(4)右侧进行特征值分解可以得到某一个求解结果为\\(\tilde{D}\\)， -->

According to equation (4), it can be seen that \\(\hat{D}\\) does not have a unique solution because for any matrix \\(V\\) satisfying \\(V^TV=I_3\\), \\(\hat{D}V\\) can satisfy equation (4).

By performing an eigenvalue decomposition on the right-hand side of equation (4), we can obtain a particular solution denoted as \\(\tilde{D}\\).

$$
\begin{aligned}
(\tilde{D}^{-1})^T(\tilde{D}^{-1})
&=\alpha U\Lambda U^T =\alpha U\Lambda^{1/2}\Lambda^{1/2} U^T \\
&=\alpha (\Lambda^{1/2} U^T)^T(\Lambda^{1/2} U^T)
\end{aligned}
$$

$$
\tilde{D}^{-1}=\sqrt{\alpha}\Lambda^{1/2} U^T
$$

<!--则其与最终解\\(\hat{D}\\)之间的关系为 -->
Therefore, the relationship between \\(\tilde{D}\\) and the final solution \\(\hat{D}\\) is

$$
\hat{D}=\tilde{D}V
$$

<!--这里的未知旋转矩阵\\(R\\)代表了磁强计和惯导之间的配准关系，无法单独通过磁测数据来确定[^3]。 -->
The unknown rotation matrix \\(R\\) here represents the alignment relationship between the magnetometer and the inertial navigation system, and cannot be determined solely through magnetic measurement data [^3].

## Alignment with Inertial Sensor

<!--\\(\boldsymbol{m}_k^m\\)是在magnetometer frame中表示的地磁矢量，其与navigation frame中的地磁矢量之间的关系为 -->
The geomagnetic vector \\(\boldsymbol{m}_k^m\\) represented in the magnetometer frame relates to the geomagnetic vector in the navigation frame as

$$
\boldsymbol{m}_k^m=R_k^{mn}\boldsymbol{m}^n=R^{mb}R_k^{bn}\boldsymbol{m}^n
\tag{5}
$$

<!--其中，\\(R^{mb}\\)表示body frame到magnetometer frame之间的姿态变换，\\(R_k^{bn}\\)则表示\\(k\\)时刻body frame在navigation frame中的姿态。

从式(5)可以看出，当载体在均匀地磁场中进行标定运动时，矢量磁场测量值的变化仅与载体的姿态变化有关[^9]。

将上一部分模型参数的求解结果和式(5)代入测量模型可得 -->


Where:

- \\(R^{mb}\\) represents the attitude transformation from the body frame to the magnetometer frame,
- \\(R_k^{bn}\\) represents the attitude of the body frame in the navigation frame at time \\(k\\).

From equation (5), it can be seen that when the carrier moves in a uniform geomagnetic field for calibration, the variation in the measured magnetic field vector depends solely on the changes in the carrier's attitude [^9].

Substituting the solved results of the previous section's model parameters and equation (5) into the measurement model, we obtain

$$
\begin{aligned}
\boldsymbol{y}_{k}^m&=\hat{D}\boldsymbol{m}_k^m+\hat{o} \\
&=\tilde{D}VR^{mb}R_k^{bn}\boldsymbol{m}^n+\hat{o} 
\end{aligned}
$$

let

$$
R=VR^{mb}
$$

then

$$
\boldsymbol{y}_{k}^m=\tilde{D}RR_k^{bn}\boldsymbol{m}^n+\hat{o}
\tag{6}
$$

<!--对于下一时刻，即\\(k+1\\)时刻的观测数据，有 -->
For the next time step \\(k+1\\), the observed data are given by

$$
\boldsymbol{y}_{k+1}^m=\tilde{D}RR_{k+1}^{bn}\boldsymbol{m}^n+\hat{o}
\tag{7}
$$

<!--结合式(6)和式(7)可得 -->
Combining equations (6) and (7), we obtain

$$
\boldsymbol{y}_{k+1}^m=\tilde{D}RR_{k+1}^{bn}
R_k^{nb}R^T\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o})
+\hat{o}
$$

let

$$
R^{b_{k+1}}_{b_k}=R_{k+1}^{bn}R_k^{nb}
$$

then we have

$$
\boldsymbol{y}_{k+1}^m=\tilde{D}R
R^{b_{k+1}}_{b_k}
R^T\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o})
+\hat{o}
\tag{8}
$$

<!--式(8)给出了两个时刻之间，由惯导提供的载体姿态增量\\(R^{b_{k+1}}_{b_k}\\)，以及三轴矢量磁强计观测之间的关联关系。 -->
Equation (8) provides the relationship between the attitude increment \\(R^{b_{k+1}}_{b_k}\\) provided by the inertial navigation system between two time steps, and the correlation between the observations of the triaxial vector magnetometer.

![img](http://sunqinxuan.github.io/images/projects-2024-07-09-img1.jpg)

## Initial Value Estimation 

If there is no suitable initial value, a nonlinear optimization problem may fail to converge to a global optimal solution. 
Therefore, an initial value is estimated for the alignment problem stated above.
Eqn.(6) can be rewritten as 

$$
\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o})=RR_k^{bn}\boldsymbol{m}^n
$$

Let 

$$
\begin{aligned}
& p_k'\triangleq\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o}) \\
& p_k\triangleq R_k^{bn}\boldsymbol{m}^n
\end{aligned}
$$

Then, a minimization problem is constructed as 

$$
\min_{R}\sum_k\|p_k'-Rp_k\|^2
\tag{9}
$$

Compute the mean values of \\(p_k'\\) and \\(p_k\\), respectively, and define 

$$
\begin{aligned}
& q_k'\triangleq p_k'-{\rm mean}(p_k') \\
& q_k\triangleq p_k-{\rm mean}(p_k)
\end{aligned}
$$

The minimization problem (9) is reconstructed as 

$$
\min_{R}\sum_k\|q_k'-Rq_k\|^2
\tag{9}
$$

Define matrix \\(H\\)[^10] as 

$$
H\triangleq\sum_k q_k'q_k^T
$$

Compute the singular value decomposition (SVD) of the matrix \\(H\\) as 

$$
H=U\Lambda V^T
$$

Then, the orthogonal matrix \\(\hat{R}\\) that minimized (9) can be calculated by [^10]

$$
\hat{R}=VU^T
$$

![img](http://sunqinxuan.github.io/images/projects-2024-07-09-img3.jpg)

## Magnetometer outside Cabin

The measurements of the magnetometer outside the cabin are denoted by the superscript \\(m'\\).

$$
\begin{aligned}
\boldsymbol{y}_{k}^m&=\hat{D}\boldsymbol{m}_k^m+\hat{o} \\
&=\tilde{D}VR^{mm'}\boldsymbol{m}^{m'}_k+\hat{o} 
\end{aligned}
$$

let 

$$
R=VR^{mm'}
$$

then

$$
\boldsymbol{y}_{k}^m=\tilde{D}R\boldsymbol{m}^{m'}_k+\hat{o}
\tag{10}
$$

Rewriting Eqn.(10) yields

$$
\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o})=R\boldsymbol{m}^{m'}_k
$$

Let 

$$
\begin{aligned}
& p_k'\triangleq\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o}) \\
& p_k\triangleq \boldsymbol{m}^{m'}_k
\end{aligned}
$$

Then, the initial value estimation method mentioned above can be applied here.
Given an initial value, the ceres optimization problem can be constructed to solve Eqn.(10).


![img](http://sunqinxuan.github.io/images/projects-2024-11-21-img1.jpg)

![img](http://sunqinxuan.github.io/images/projects-2024-11-21-img2.jpg)






## Related Links 

code:
- [magnetic_compensation](https://github.com/sunqinxuan/magnetic_compensation)

## references

[^1]: [A Non-linear, Two-step Estimation Algorithm for Calibrating Solid-state Strapdown Magnetometers](https://users.soe.ucsc.edu/~elkaim/Documents/TwoVec.pdf)

[^2]: [Calibration of Strapdown Magnetometers in the Magnetic Field Domain](https://ascelibrary.org/doi/10.1061/%28ASCE%290893-1321%282006%2919%3A2%2887%29)

[^3]: [Calibration of a magnetometer in combination with inertial sensors](https://ieeexplore.ieee.org/document/6289882)

[^4]: [Sensor Fusion and Calibration of Inertial Sensors, Vision, Ultra-Wideband and GPS](https://user.it.uu.se/~thosc112/team/hol2011.pdf)

[^5]: [Magnetometer calibration using inertial sensors](https://ieeexplore.ieee.org/document/7470259)

[^6]: [Least Squares Ellipsoid Specific Fitting](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=1290055)

[^7]: [Q LI (2024). Ellipsoid Fitting, MATLAB Central File Exchange. 检索来源 2024/6/3.](https://ww2.mathworks.cn/matlabcentral/fileexchange/23377-ellipsoid-fitting)

[^8]: [A way to calibrate a magnetometer](https://teslabs.com/articles/magnetometer-calibration/)

[^9]: [On Misalignment between Magnetometer and Inertial Sensors](https://ieeexplore.ieee.org/document/7496814)

[^10]: [Least-Squares Fitting of Two 3-D Point Sets](https://ieeexplore.ieee.org/document/4767965)