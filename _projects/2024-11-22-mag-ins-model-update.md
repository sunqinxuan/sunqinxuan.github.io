---
title: 'Simultaneous Calibration and Localization'
collection: projects
type: "Research Project"
excerpt: '...'
venue: "Geomagnetic Navigation"
permalink: /projects/2024-11-22-compensation
date: 2024-11-22
location: "Beijing, China"
---

## magnetometer-inertail navigation system 

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

## Sensor Calibration with Known Magnetic Field

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
\tag{1}
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

## Simultaneous Calibration and Localization

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

## Particle Filter Algorithm for Calibration and Localization

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
\tilde(\omega)^i_k=\omega^i_{k-1}\mathcal{N}(y_k;y^i_k,\Sigma^i_k)
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


## references

[^1]: [Localization With Magnetic Field Distortions and Simultaneous Magnetometer Calibration](https://ieeexplore.ieee.org/document/9195842)
-->