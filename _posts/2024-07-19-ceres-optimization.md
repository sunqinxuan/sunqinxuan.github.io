---
title: 'Ceres Optimization Problem Construction for Mag-INS Alignment'
date: 2024-04-25
permalink: /posts/research-journal-2024-04-25-3d-ndt
excerpt: '...'
tags:
  - research journal
---

The problem to be solved is presented in detail [here](https://sunqinxuan.github.io/projects/2024-07-09-compensation).

The constrained non-linear least squares problem can be modeled by 

$$
\begin{aligned}
\min_R\quad&
\frac{1}{2}
\sum_k
\rho_k
\left(
\left\|
f_k(R)
\right\|^2
\right) \\
\rm{s}.\rm{t}.\quad& R\in\mathbb{SO}(3)
\end{aligned}
$$

where 

$$
f_k(R)=
\boldsymbol{y}_{k+1}^m-
\left(\tilde{D}R
R^{b_{k+1}}_{b_k}
R^T\tilde{D}^{-1}(\boldsymbol{y}_{k}^m-\hat{o})
+\hat{o}\right)
$$

is the `CostFunction` that depends on the **parameter block** parameterizing the rotation matrix \\(R\\).
And \\(\rho_k(\|f_k(R)\|^2)\\) is known as a **residual block**.

\\(\rho_k\\) is a `LossFunction`. A `LossFunction` is a scalar valued function that is used to reduce the influence of outliers on the solution of non-linear least squares problems.





