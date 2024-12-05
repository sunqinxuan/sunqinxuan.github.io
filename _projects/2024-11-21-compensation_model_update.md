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
{\rm vec}(C)
\end{bmatrix}
$$

The sensor model can be rewritten as (drop the superscript for convenience)

$$
\boldsymbol{y}_{k}=h(\boldsymbol{m}_k)\theta+\boldsymbol{e}_{k}
\tag{1}
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

## Kalman Filter 

The Kalman filter can be used to the LSE to estimate the parameters. 
This is particularly helpful when the estimation is performed online in a recursive manner. 
The prediction model for a KF can be described by a time discrete random walk

$$
\theta_k=\theta_{k-1}+{\rm w}_{k-1}^\theta
$$

with 

$$
{\rm w}_{k}^\theta\sim\mathcal{N}(0,R_k)
$$

The measurement model is decribed by (1), with the process noise 

$$
e_{k}\sim\mathcal{N}(0,Q_k)
$$

The Kalman Filter process is summarized by 

$$
\begin{aligned}
&\bar{\theta}_k=\theta_{k-1} \\
&\bar{\Sigma}_k=\Sigma_{k-1}+R_k \\
&K_k=\bar{\Sigma}_k h(m_k)^T
\left(h(m_k)\bar{\Sigma}_kh(m_k)^T+Q_k\right)^{-1} \\
&\theta_k=\bar\theta_k+K_k\left(y_k-h(m_k)\bar\theta_k\right) \\
&\Sigma_k=\left(I-K_kh(m_k)\right)\bar\Sigma_k
\end{aligned}
$$

The Kalman filter intrinsically accounts for the observability of the parameters. 
If the measurements are correlated and the Kalman filter is not already converged, the state covariance is large indicating large uncertainty in the estimation and vice versa. [^1]

<!--
![img](http://sunqinxuan.github.io/images/projects-2024-11-21-img1.jpg)

![img](http://sunqinxuan.github.io/images/projects-2024-11-21-img2.jpg)
-->


## Simultaneous Calibration and Localization

### magnetometer-inertial navigation system 

The measurement model is given by

$$
\boldsymbol{y}_{k}^m=D\boldsymbol{m}_k^m+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

$$
\boldsymbol{y}_{k}^m=DR^{mb}R_k^{bn}\boldsymbol{m}_k^{n}+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

let 

$$
C=DR^{mb}
$$

then

$$
\boldsymbol{y}_{k}^m=CR_k^{bn}\boldsymbol{m}_k^{n}+\boldsymbol{o}+\boldsymbol{e}_{k}^m
$$

### Sensor Calibration with Known Magnetic Field

Define the compensation model parameters

$$
\theta=
\begin{bmatrix}
o \\
{\rm vec}(C)
\end{bmatrix}
$$

Define 

$$
m_k^b=R_k^{bn}m_k^n\doteq
\begin{bmatrix}
x^b_k & y^b_k & z^b_k 
\end{bmatrix}^T
$$

The sensor model can be rewritten as (drop the superscript for convenience)

$$
{y}_{k}=h(m_k^b)\theta+{e}_{k}
$$

where 

$$
h(\boldsymbol{m}_k)=
\begin{bmatrix}
I_{3\times3} &
x^b_kI_{3\times3} &
y^b_kI_{3\times3} &
z^b_kI_{3\times3}
\end{bmatrix}
$$

### Simultaneous Calibration and Localization

The state of the system is defined as 

$$
x_k=
\begin{bmatrix}
p_k^T & q_k^T & \theta_k^T
\end{bmatrix}^T
\doteq
\begin{bmatrix}
d_k^T & \theta_k^T
\end{bmatrix}^T
$$

Then the time evolution of the state can be represented by 

$$
x_k=
\begin{bmatrix}
f(d_{k-1},{\rm w}^d_{k-1}) \\
\theta_{k-1}+{\rm w}_{k-1}^\theta
\end{bmatrix}
$$

with 

$$
{\rm w}_{k}^d\sim\mathcal{N}(0,R_k^d)
$$

$$
{\rm w}_{k}^\theta\sim\mathcal{N}(0,R_k^\theta)
$$

The measurement model is decribed by 

$$
{y}_{k}=h(R_k^{bn}m(p_k))\theta+{e}_{k}
$$

with

$$
e_{k}\sim\mathcal{N}(0,Q_k)
$$

and \\(m(\cdot)\\) represents the map of magnetic field.

### Particle Filter Algorithm for Calibration and Localization

For each particle \\(i\\), perform the following steps:

- Sample process noise 

$$
{\rm w}^{d,i}_{k-1}\sim\mathcal{N}(0,R^d)
$$

- Particle filter time update (for nonlinear part of the state)

$$
d^i_k=f(d^i_{k-1},{\rm w}^{d,i}_{k-1})
$$

- Kalman covariance time update (for linear part of the state)

$$
\bar{P}^i_k=P^i_{k-1}+R^\theta
$$

- Kalman measurement data prediction 

$$
H^i_k=h(R_k^{bn}m(p_k))
$$

$$
y^i_k=H^i_k\theta^i_{k-1}
$$

$$
\Sigma^i_k=Q_k+H^i_k \bar{P}^i_k {H^i_k}^T
$$

- Weight update

$$
\tilde\omega^i_k=\omega^i_{k-1}\mathcal{N}(y_k;y^i_k,\Sigma^i_k)
$$

- Kalman gain computtion

$$
K^i_k=\bar{P}^i_k{H^i_k}^T{\Sigma^i_k}^{-1}
$$

- Kalman state and covariance update 

$$
\theta^i_k=\theta^i_{k-1}+K^i_k(y_k-y^i_k)
$$

$$
P^i_k=\bar{P}^i_k-K^i_k\Sigma^i_k{K^i_k}^T
$$

<!--
![img](http://sunqinxuan.github.io/images/projects-2024-11-22-img1.jpg)

![img](http://sunqinxuan.github.io/images/projects-2024-11-22-img2.jpg)

![img](http://sunqinxuan.github.io/images/projects-2024-11-22-img3.jpg)
-->

<!--
## Related Links 

code:
- [magnetic_compensation](https://github.com/sunqinxuan/magnetic_compensation)
-->

## references

[^1]: [Localization With Magnetic Field Distortions and Simultaneous Magnetometer Calibration](https://ieeexplore.ieee.org/document/9195842)