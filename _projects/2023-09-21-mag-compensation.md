---
title: "基于Tolles-Lawson模型的航磁补偿系统"
collection: projects
type: "Project"
permalink: /projects/2023-09-21-mag-compensation
venue: "ZJLab"
date: 2023-09-21
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

## 方案

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

### 基于总量约束的矢量磁补偿

基于测量总场矢量与地磁场矢量以及干扰场矢量的关系式，推导总场的标量信号\\|(\vec{B}_t|\\)
和地磁场标量信号\\|(\vec{B}_e|\\)之间的关系。

$$
\vec{B}_t=\vec{B}_e+\vec{B}_a
$$


### 基于分量约束的矢量磁补偿














<img src="https://sunqinxuan.github.io/images/projects-2023-09-21-img1.jpg" alt="architecture" />

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










