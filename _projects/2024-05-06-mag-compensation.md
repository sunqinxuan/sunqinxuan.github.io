---
title: "基于Tolles-Lawson模型的航磁补偿系统"
collection: projects
type: "Project"
permalink: /projects/2024-05-06-mag-compensation
venue: ""
date: 2024-05-06
location: "Beijing, China"
---

基于Tolles-Lawson模型的航磁补偿系统

## 背景

详情可参阅[Signal Enhancement for Magnetic Navigation Challenge Problem](https://magnav.mit.edu/)。

> The best submission to date was submitted by: Ling-Wei Kong, Cheng-Zhen Wang, and Ying-Cheng Lai from Arizona State University ([submission](https://github.com/lw-kong/MagNav)).

### dataset版本问题

dataset有两个版本：版本一和版本二。

两个版本所采集数据是相同的，但是文件组织不同，包括：数据命名、数据存储顺序等。

使用时须注意。

[版本一](https://zenodo.org/record/6327685)

![img](http://sunqinxuan.github.io/images/projects-2023-09-21-img01.png)

[版本二](https://zenodo.org/record/4271804#.YnWQuIdBxD8)

![img](http://sunqinxuan.github.io/images/projects-2023-09-21-img02.png)

## 基于Tolles-Lawson模型的矢量磁补偿方案

舱内测量磁场由两部分组成，可以表示为

$$
\vec{B}_t=\vec{B}_e+\vec{B}_a
$$

其中
- \\(\vec{B}_t\\)表示舱内实测磁场矢量；
- \\(\vec{B}_e\\)表示地球磁场，包括主磁场、异常场、日变场三部分；
- \\(\vec{B}_a\\)为未知的载体干扰磁场。

根据Tolles-Lawson模型的假设，载体干扰磁场同固定磁场、感应磁场和涡流磁场组成，即

$$
\vec{B}_a=\vec{B}_{perm}+\vec{B}_{ind}+\vec{B}_{eddy}
$$

固定磁场可以建模为

$$
\vec{B}_{perm}=\boldsymbol{a}=
\begin{bmatrix}
a_1 \\ a_2\\ a_3
\end{bmatrix}
$$

其中包含3个未知系数，这些固定磁矩项表示各种铁磁载体组件的几乎恒定的永久磁化，包括载体本身和载体内的物品。除非载体的配置或内容发生变化，否则这些项不会改变。

感应磁矩项可以表示为

$$
\vec{B}_{ind}=\boldsymbol{b}\vec{B}_t=
\begin{bmatrix}
b_{11} & b_{12} & b_{13} \\
b_{21} & b_{22} & b_{23} \\
b_{31} & b_{32} & b_{33} 
\end{bmatrix}
\vec{B}_t
$$

其中包含9个未知系数。感应磁矩项表示地球场在磁性易感载体组件中诱发出的次级磁场。载体和地球场的相对方向决定了感应磁化的大小和方向。由于飞机结构主要由非磁性铝合金构成，感应磁场的主要来源是飞机发动机。

注意：这里有一个近似，使用可观测量\\(\vec{B}_t\\)代替了未知量\\(\vec{B}_e\\)，详情可见[这里](https://sunqinxuan.github.io/posts/research-journal-2024-04-17-tl-model)。

涡流磁矩表示为

$$
\vec{B}_{eddy}=\boldsymbol{c}\dot{\vec{B}}_t=
\begin{bmatrix}
c_{11} & c_{12} & c_{13} \\
c_{21} & c_{22} & c_{23} \\
c_{31} & c_{32} & c_{33} 
\end{bmatrix}
\dot{\vec{B}}_t
$$

其中包含9个未知系数。涡流项表示由于时变的地球场（相对于载体）与电导性载体组件相互作用而引起的电流环。与固定磁场和感应磁场不同，涡流磁场取决于地球磁通量通过这些组件的时间变化率，例如载体外壳等。由涡流产生的磁场遵循伦兹定律，与产生它们的磁场相对抗，类似于在均匀磁场中旋转的线圈中产生电流的原理。

基于以上参数化形式，干扰磁场可以表示为

$$
\vec{B}_a=\boldsymbol{a}+\boldsymbol{b}\vec{B}_t+\boldsymbol{c}\dot{\vec{B}}_t
$$

其中，参数\\(\boldsymbol{a,b,c}\\)总共包含21个系数，但由于感应磁矩矩阵的对称性，在标准的Tolles-Lawson模型中，总共有18个未知系数。

$$
\vec{B}_a=
\begin{bmatrix}
\beta_1 \\ \beta_2\\ \beta_3
\end{bmatrix}
+
\begin{bmatrix}
\beta_4 & \beta_5 & \beta_6 \\
\beta_5 & \beta_7 & \beta_8 \\
\beta_6 & \beta_8 & \beta_9 
\end{bmatrix}
\vec{B}_t
+
\begin{bmatrix}
\beta_{10} & \beta_{11} & \beta_{12} \\
\beta_{13} & \beta_{14} & \beta_{15} \\
\beta_{16} & \beta_{17} & \beta_{18} 
\end{bmatrix}
\dot{\vec{B}}_t
$$

其中，\\(\boldsymbol{\beta}=[\beta_1,\beta_2,\cdots,\beta_{18}]^T\\)表示待求解的Tolles-Lawson模型参数。

### 基于总量约束的模型参数求解

基于测量总场矢量与地磁场矢量以及干扰场矢量的关系式，推导总场的标量信号和地磁场标量信号之间的关系。

$$
\vec{B}_t=\vec{B}_e+\vec{B}_a
$$

$$
\begin{aligned}
|\vec{B}_e|^2&=\vec{B}_e^T\vec{B}_e=(\vec{B}_t-\vec{B}_a)^T(\vec{B}_t-\vec{B}_a)\\
&=\vec{B}_t^T\vec{B}_t-2\vec{B}_t^T\vec{B}_a+\vec{B}_a^T\vec{B}_a
\end{aligned}
$$

$$
\begin{aligned}
|\vec{B}_e|&=\sqrt{|\vec{B}_t|^2-2\vec{B}_t^T\vec{B}_a+|\vec{B}_a|^2}\\
&=|\vec{B}_t|\sqrt{1-2\frac{\vec{B}_t^T\vec{B}_a}{|\vec{B}_t|^2}+\frac{|\vec{B}_a|^2}{|\vec{B}_t|^2}}
\end{aligned}
$$

由于

$$
|\vec{B}_a|^2\ll|\vec{B}_t|^2
$$

可进行以下近似

$$
|\vec{B}_e|\approx|\vec{B}_t|
\sqrt{1-\frac{\vec{B}_t^T\vec{B}_a}{|\vec{B}_t|^2}}
$$

使用泰勒展开进行线性化可得

$$
|\vec{B}_e|\approx|\vec{B}_t|-
\frac{\vec{B}_t^T\vec{B}_a}{|\vec{B}_t|}
$$

令测量总磁场的方向余弦（可由矢量磁强计的测量结果获得）为

$$
\hat{B}_t=\frac{\vec{B}_t}{|\vec{B}_t|}
$$

则有

$$
|\vec{B}_e|\approx|\vec{B}_t|-\vec{B}_a^T\hat{B}_t
$$

其中\\(\vec{B}_a^T\hat{B}_t\\)为载体干扰磁场在总场方向上的投影，具体计算为

$$
\vec{B}_a^T\hat{B}_t=(\boldsymbol{a}+\boldsymbol{b}\vec{B}_t+\boldsymbol{c}\dot{\vec{B}}_t)^T\hat{B}_t
$$

则基于总量的约束关系可以表示为

$$
|\vec{B}_e|\approx|\vec{B}_t|-
\left(
\hat{B}_t^T
\begin{bmatrix}
\beta_1 \\ \beta_2\\ \beta_3
\end{bmatrix}+
\hat{B}_t^T
\begin{bmatrix}
\beta_4 & \beta_5 & \beta_6 \\
\beta_5 & \beta_7 & \beta_8 \\
\beta_6 & \beta_8 & \beta_9 
\end{bmatrix}
\vec{B}_t+
\hat{B}_t^T
\begin{bmatrix}
\beta_{10} & \beta_{11} & \beta_{12} \\
\beta_{13} & \beta_{14} & \beta_{15} \\
\beta_{16} & \beta_{17} & \beta_{18} 
\end{bmatrix}
\dot{\vec{B}}_t
\right)
$$

<img src="https://sunqinxuan.github.io/images/projects-2024-05-06-img1.png" alt="architecture" />

对上式进行整理，可以得到总量约束关系相对于模型参数的矢量形式

$$
|\vec{B}_t|-|\vec{B}_e|=\vec{\delta}^T\boldsymbol{\beta}
$$

其中18维系数向量\\(\vec{\delta}\\)为

$$
\vec{\delta}=
\begin{bmatrix}
\hat{B}_x \\
\hat{B}_y \\
\hat{B}_z \\
\hat{B}_x\vec{B}_x \\
\hat{B}_x\vec{B}_y \\
\hat{B}_x\vec{B}_z \\
\hat{B}_y\vec{B}_y \\
\hat{B}_y\vec{B}_z \\
\hat{B}_z\vec{B}_z \\
\hat{B}_x\dot{\vec{B}}_x \\
\hat{B}_x\dot{\vec{B}}_y \\
\hat{B}_x\dot{\vec{B}}_z \\
\hat{B}_y\dot{\vec{B}}_x \\
\hat{B}_y\dot{\vec{B}}_y \\
\hat{B}_y\dot{\vec{B}}_z \\
\hat{B}_z\dot{\vec{B}}_x \\
\hat{B}_z\dot{\vec{B}}_y \\
\hat{B}_z\dot{\vec{B}}_z \\
\end{bmatrix}
$$

假设长度为\\(N\\)的时间序列，每个时刻的\\(\vec{\delta}\\)可以组成\\(N\times18\\)维矩阵\\(\boldsymbol{A}\\)

$$
\boldsymbol{A}=
\begin{bmatrix}
\vec{\delta}_1 \\
\vdots\\
\vec{\delta}_N \\
\end{bmatrix}
$$

并且令

$$
\boldsymbol{B}_t=
\begin{bmatrix}
|\vec{B}_t|_1 \\
\vdots\\
|\vec{B}_t|_N \\
\end{bmatrix}
$$

$$
\boldsymbol{B}_e=
\begin{bmatrix}
|\vec{B}_e|_1 \\
\vdots\\
|\vec{B}_e|_N \\
\end{bmatrix}
$$

则有

$$
\boldsymbol{B}_t-\boldsymbol{B}_e=\boldsymbol{A}\boldsymbol{\beta}
$$

上式可通过最小二乘方法进行求解

$$
\boldsymbol{\beta}=(\boldsymbol{A}^T\boldsymbol{A})^{-1}\boldsymbol{A}^T(\boldsymbol{B}_t-\boldsymbol{B}_e)
$$


### 基于分量约束的模型参数求解

直接考虑基于分量的约束关系

$$
\vec{B}_e=\vec{B}_t-
\left(
\begin{bmatrix}
\beta_1 \\ \beta_2\\ \beta_3
\end{bmatrix}+
\begin{bmatrix}
\beta_4 & \beta_5 & \beta_6 \\
\beta_5 & \beta_7 & \beta_8 \\
\beta_6 & \beta_8 & \beta_9 
\end{bmatrix}
\vec{B}_t+
\begin{bmatrix}
\beta_{10} & \beta_{11} & \beta_{12} \\
\beta_{13} & \beta_{14} & \beta_{15} \\
\beta_{16} & \beta_{17} & \beta_{18} 
\end{bmatrix}
\dot{\vec{B}}_t
\right)
$$

根据前文所述的模型参数形式，对上式进行整理，得到基于分量关系的矢量形式

$$
\vec{B}_t-\vec{B}_e=\Lambda\boldsymbol{\beta}
$$

其中\\(3\times18\\)维系数矩阵\\(\Lambda\\)为

$$
\Lambda=
\begin{bmatrix}
\lambda_1 & \lambda_2 & \lambda_3
\end{bmatrix}
$$

其中

$$
\lambda_1=\boldsymbol{I}_{3\times3}
$$

$$
\lambda_2=
\begin{bmatrix}
\vec{B}_x & \vec{B}_y & \vec{B}_z & 0 & 0 & 0 \\
0 & \vec{B}_x & 0 & \vec{B}_y & \vec{B}_z & 0 \\
0 & 0 & \vec{B}_x & 0 & \vec{B}_y & \vec{B}_z \\
\end{bmatrix}
$$

$$
\lambda_3=
\begin{bmatrix}
\dot{\vec{B}}_x & \dot{\vec{B}}_y & \dot{\vec{B}}_z & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & \dot{\vec{B}}_x & \dot{\vec{B}}_y & \dot{\vec{B}}_z & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & \dot{\vec{B}}_x & \dot{\vec{B}}_y & \dot{\vec{B}}_z \\
\end{bmatrix}
$$

类似地，对于长度为\\(N\\)的时间序列，可以构成\\(3N\times18\\)维系数矩阵\\(\boldsymbol{A}\\)

$$
\boldsymbol{A}=
\begin{bmatrix}
\Lambda_1 \\
\vdots\\
\Lambda_N \\
\end{bmatrix}
$$

并且令

$$
\boldsymbol{B}_t=
\begin{bmatrix}
\vec{B}_{t1} \\
\vdots\\
\vec{B}_{tN} \\
\end{bmatrix}
$$

$$
\boldsymbol{B}_e=
\begin{bmatrix}
\vec{B}_{e1} \\
\vdots\\
\vec{B}_{eN} \\
\end{bmatrix}
$$

则有

$$
\boldsymbol{B}_t-\boldsymbol{B}_e=\boldsymbol{A}\boldsymbol{\beta}
$$

同样可通过最小二乘方法进行求解

$$
\boldsymbol{\beta}=(\boldsymbol{A}^T\boldsymbol{A})^{-1}\boldsymbol{A}^T(\boldsymbol{B}_t-\boldsymbol{B}_e)
$$

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img1.jpg" alt="architecture" />

### 矢量磁补偿

在使用基于总量约束或分量约束的模型参数求解方法得到\\(\boldsymbol{\beta}\\)后，矢量磁补偿过程可以通过下式进行

$$
\vec{B}_e=\vec{B}_t-\Lambda\boldsymbol{\beta}
$$

## 地磁场标定信号的获取

在前文所述模型参数求解的过程中，\\(\vec{B}_t\\)与\\(\boldsymbol{\beta}\\)均来自标量及矢量磁强计的测量值，而地磁场\\(\vec{B}_e\\)的来源问题需要进一步讨论。

### 来源一：舱外安装磁强计（真值）

在标定飞行（磁补偿模型拟合数据采集）过程中，在舱外安装标量和矢量磁测组，同步采集每一时刻的地磁场真值，用于模型参数的求解，以及补偿精度的测试。

>Comparing to the map values is not done in most of this work, since the tail stinger measurement is considered to be a better “truth” measurement as it was measured at the same time and avoids map errors.<br>
ref. [A. R. Gnadt, “Advanced Aeromagnetic Compensation Models for Airborne Magnetic Anomaly Navigation,” Massachusetts Institute of Technology, 2022. ](https://dspace.mit.edu/handle/1721.1/145137)

### 来源二：高精度地磁基准图

目前不具备适用性，并且无法获取矢量地磁场信息。

> The map-based calibration, is less common in the literature,
but more intuitive. This calibration method flies
through a high-quality magnetic anomaly map (with GPS)
and calibrates the system measurements such that they most
closely match the magnetic anomaly map. This method
requires highly accurate magnetic anomaly maps but can
produce the best results given the navigation system is also
using this map to navigate. This method cannot produce
calibration errors smaller than the errors which exist in the
magnetic map-products being used.<br>
ref. [A. J. Canciani, "Magnetic Navigation on an F-16 Aircraft Using Online Calibration," in IEEE Transactions on Aerospace and Electronic Systems, vol. 58, no. 1, pp. 420-434.](https://ieeexplore.ieee.org/document/9506809)


### 来源三：bpf "trick"

在标定飞行过程中，飞行区域选择地磁场变化较小的区域，并使无人机做各类机动动作，然后通过带通滤波器，滤除地磁场信号。

但这一方案只适用于“基于总量约束的参数求解”方法。

> The “trick” is to use a bandpass finite impulse response filter (bpf).<br>
The passband frequency range for the bandpass filter is carefully selected in order
to remove nearly all of the earth field while keeping much of the aircraft field. In
practice, a passband of 0.1-0.9 Hz has been found to perform well, since in this
range the frequency content of the aircraft dominates the magnetic signal.<br>
ref. [A. R. Gnadt, “Advanced Aeromagnetic Compensation Models for Airborne Magnetic Anomaly Navigation,” Massachusetts Institute of Technology, 2022. ](https://dspace.mit.edu/handle/1721.1/145137)

## 标定飞行




## 实验

ensemble rmse on 1003.020000 = 9.241940

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img2.jpg" alt="architecture" />

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img3.jpg" alt="architecture" />

ensemble rmse on 1003.080000 = 8.374712

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img4.jpg" alt="architecture" />

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img5.jpg" alt="architecture" />

ensemble rmse on 1003.040000 = 10.538149

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img6.jpg" alt="architecture" />

<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img7.jpg" alt="architecture" />

## 相关链接

专利：
- [基于矢量数据的地磁导航方法、装置以及电子设备](https://sunqinxuan.github.io/publication/CN117213472A)

代码：
- [a modified version with c++ implementation of the TL-model](https://github.com/sunqinxuan/magnav)










