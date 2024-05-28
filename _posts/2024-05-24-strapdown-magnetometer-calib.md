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

[5] [Magnetometer calibration using inertial sensors](https://ieeexplore.ieee.org/document/7470259)

目前所使用的MIT的基于TL模型的补偿方法，线性最小二乘参数估计方法比较弱，对于噪声和数据异常等情况的鲁棒性较差，而且在建模的时候没有包含飞机姿态参数。另外，由于真值的缺失，使得目前以最小化测量值和真值残差的拟合方法难以顺利进行。

针对上述问题，我对磁强计加惯导的捷联系统整体标定和补偿的方法进行了一些调研。对于干扰磁场模型，大多数文献中都是采用了硬磁干扰加软磁干扰的建模方式，与我们目前所使用的TL模型是一致的。由于没有真值做为参考，因此使用椭圆拟合的原理，对模型参数进行计算，比如文献[1][2]中，使用变量替换的方法，将椭圆方程中的非线性约束转化为线性约束，完成求解后再转换回去。而文献[3][4]的估计模型中，不仅包含了标定参数和补偿参数，还包含传感器每个时刻的姿态，是个高度非线性的问题，通过最大似然估计进行求解。但是这样的求解方法需要给一个离最优解相对较近的初值，不然可能不收敛，因此使用椭圆约束和一些其他约束条件，先一步进行初值的估计。关于这一部分，我还没有太理清。但我觉得这样的方法对于我想要将飞行姿态考虑进行的想法还是比较有启发的，所以准备进一步研究一下。

不考虑任何误差与干扰的情况下，在均匀磁场转运载体时，测量磁场矢量的模长不受姿态影响，在三维空间中其轨迹落在球面上。当受到误差和干扰的影响时，理论上磁场测量矢量的轨迹形成一个椭球面。

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

## Magnetometer measurement model

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

- \\(\boldsymbol{o}_{hi}\\)为载体干扰磁场中的固定磁场向量（hard-iron effects）；

- \\(R_{im}\\)表示磁强计与惯导之间的姿态偏差。

进一步整理后，得到最终的测量模型：

$$
\boldsymbol{y}_{m,k}^b=DR_k^{bn}\boldsymbol{m}^n+\boldsymbol{o}+\boldsymbol{e}_{m,k}^b
$$

其中，

$$
D=C_{sc}C_{no}C_{si}R_{im}
$$

$$
\boldsymbol{o}=C_{sc}C_{no}\boldsymbol{o}_{hi}+\boldsymbol{o}_{zb}
$$

上述模型在目前的研究中是比较通用的，该模型能够成立的一些假设条件：

- 干扰磁场须来自于与传感器刚性固连的设备或者载体平台；

- 标定数据采集时，外部磁场（地磁场）是均匀不变的。

在本课题中，以上两个假设均可以视作已满足。

## Ellipse fit

首先，将局部均匀地磁场矢量进行归一化，即

$$
\|{m}^n\|_2=1
$$

则有以下等式成立

$$
\|{m}^n\|_2^2-1=\|R_k^{nb}{m}_k^b\|_2^2-1=\|{m}_k^b\|_2^2-1=0
$$

展开可得椭球约束方程：

$$
\begin{aligned}
0&=\|D^{-1}({y}_{m,k}-{o}-{e}_{m,k})\|_2^2-1 \\
&\approx{y}_{m,k}^TA{y}_{m,k}+{b}^T{y}_{m,k}+c
\end{aligned}
$$

其中

$$
A=D^{-T}D^{-1} \tag{1}
$$

$$
{b}^T=-2{o}^TD^{-T}D^{-1} \tag{2}
$$

$$
c={o}^TD^{-T}D^{-1}{o}-1 \tag{3}
$$

对椭球方程进行整理可得到

$$
M\eta\approx0
$$

$$
M=
\begin{bmatrix}
y_{m,1}\otimes y_{m,1} & y_{m,1} & 1 \\
y_{m,2}\otimes y_{m,2} & y_{m,2} & 1 \\
\vdots & \vdots & \vdots \\
y_{m,N}\otimes y_{m,N} & y_{m,N} & 1 \\
\end{bmatrix}
$$

$$
\eta=
\begin{bmatrix}
{\rm vec}A \\
b\\
c
\end{bmatrix}
$$

其中\\(\otimes\\)表示[Kronecker product](https://en.wikipedia.org/wiki/Kronecker_product)，\\(\rm vec\\)表示[vectorization operator](https://en.wikipedia.org/wiki/Vectorization_(mathematics))。

可以通过SVD分解对方程进行求解，求解结果\\(\hat{\eta}_s\\)为矩阵\\(M\\)的最小奇异值对应的向量，\\(\hat{\eta}_s\\)满足模长为1的约束。需要注意的是，对于任意\\(\alpha\in\mathbb{R}\\)，\\(\alpha\hat{\eta}_s\\)同样可以满足该椭圆方程。

假设我们最终的求解结果为

$$
\hat{\eta}=\alpha\hat{\eta}_s
$$

即

$$
\begin{bmatrix}
{\rm vec}\hat{A}\\
\hat{b}\\
\hat{c}
\end{bmatrix}
=
\begin{bmatrix}
{\rm vec}(\alpha\hat{A}_s)\\
\alpha\hat{b}_s\\
\alpha\hat{c}_s
\end{bmatrix}
$$

根据式(3)提供的约束，可以确定变量\\(\alpha\\)的值。

$$
\begin{aligned}
1&={o}^TD^{-T}D^{-1}{o}-c \\
&=\frac{1}{4}b^TA^{-1}b-c \\
&=\alpha(\frac{1}{4}b_s^TA_s^{-1}b_s-c_s)
\end{aligned}
$$

可以得到 

$$
\alpha=(\frac{1}{4}b_s^TA_s^{-1}b_s-c_s)^{-1}
$$

<font color=blue>
这里有个问题，式(3)成立的前提是\\(\|{m}^n\|_2=1\\)，即对地磁场进行了归一化。
这一点会带来什么影响呢？还没想明白。
</font>

接下来，根据式(1)-(3)，对模型参数的初值估计结果\\(\hat{D}_0,\hat{o}_0\\)进行反解。

$$
\hat{D}_0^{-T}\hat{D}_0^{-1}=\alpha\hat{A}_s \tag{4}
$$

$$
\hat{o}_0=-\frac{1}{2}\hat{A}_s^{-1}\hat{b}_s
$$

根据式(4)可以看出，\\(\hat{D}_0\\)没有唯一解，因为对于任意满足\\(U^TU=I_3\\)的矩阵\\(U\\)，\\(\hat{D}_0U\\)都可以使式(4)成立。

假设通过Cholesky分解由式(4)得到某一个求解结果为\\(\tilde{D}_0\\)，则其与最终解\\(\hat{D}_0\\)之间的关系为

$$
\hat{D}_0=\tilde{D}_0R
$$

根据文献[3][4][5]中的描述，这里的未知旋转矩阵\\(R\\)代表了磁强计和惯导之间的配准关系，无法单独通过磁测数据来确定。

<!--

## Problem formulation

在文献[5]中，磁强计-惯导捷联系统标定问题被建模为未知模型参数下的传感器姿态估计问题。

>Our magnetometer calibration algorithm is formulated as a problem of determining the sensor's orientation in the presence of unknown model parameters.

令系统状态\\(x_t\\)表示\\(t\\)时刻的传感器姿态，则对应非线性系统状态模型可以表示为：

$$
x_{t+1}=f_t(x_t,\omega_t,e_{\omega,t},\theta),
$$

$$
y_t=
\begin{bmatrix}
y_{a,t}\\
y_{m,t}
\end{bmatrix}=
\begin{bmatrix}
h_{a,t}(x_t)\\
h_{m,t}(x_t,\theta)
\end{bmatrix}
+e_t(\theta)
$$


## 标定算法流程

![img](http://sunqinxuan.github.io/images/posts-research-journal-2024-05-24-img2.png)


## MAXIMUM LIKELIHOOD FORMULATION

在补偿之前，磁测量数据分布在一个椭球面上，而在补偿后，理论上磁测量数据将分布在一个球面上。

以下将省略表示body frame的上标\\(b\\)。

综合上节内容，总的测量模型为

$$
\boldsymbol{y}_{m,k}=DR_k^{bn}\boldsymbol{m}^n+\boldsymbol{o}+\boldsymbol{e}_{m,k}
$$

定义模型参数\\(\boldsymbol\theta\\)为

$$
\boldsymbol\theta=\{D,\boldsymbol{o},\boldsymbol{m}^n,\{R^{bn}_k\}_{k=1}^K\}
$$

其中

$$
D\in\mathbb{R}^{3\times3}, \boldsymbol{o}\in\mathbb{R}^{3},
$$

$$
\boldsymbol{m}^n\in\{\mathbb{R}^{3}:\|\boldsymbol{m}^n\|_2^2=1,m_y^n=0\},
$$

$$
\{R^{bn}_k\}_{k=1}^K\in\mathbb{SO}(3)
$$

<font color=blue>
这里有个问题，载体或传感器相对于navigation frame的姿态\\(R^{bn}_k\\)是无法直接获取到的，需要利用惯导等额外测量进行估计。
</font>

假设噪声项为独立分布高斯白噪声，即

$$
\boldsymbol{e}_{m,k}\sim\mathcal{N}(0,\Sigma_m)
$$

通过对以下最大似然问题的求解，得到参数\\(\boldsymbol\theta\\)的最优估计：

$$
\hat{\boldsymbol{\theta}}_{ML}=\arg\max_{\boldsymbol{\theta}}
p_\theta(\boldsymbol{y}_{1:K})
$$

其中

$$
\boldsymbol{y}_{1:K}=\{\boldsymbol{y}_{m,1},\boldsymbol{y}_{m,2},\cdots,\boldsymbol{y}_{m,K}\}
$$

在独立分布高斯白噪声的假设下，最大似然估计问题可以转化为

$$
\hat{\boldsymbol{\theta}}_{ML}=\arg\min_{\boldsymbol{\theta}}
\frac{1}{2}
\sum_{k=1}^K
\|\boldsymbol{e}_{m,k}\|^2_{\Sigma_m^{-1}}
$$

在文献[3][4]中，目标函数中还有另外一项，是垂直向地心方向的磁测观测模型，但在本课题中，这个观测数据似乎无法直接获取，因此忽略这一项。

## INITIAL ESTIMATE

为了保证ML问题的求解能够收敛到最优解，利用椭圆约束等条件，对参数进行初值的估计。

-->























