---
title: 'ICP实现'
date: 2013-10-30
permalink: /posts/research-journal-2013-10/
tags:
  - research journal
---

research journal, 2013.11.

## 2013年11月4日星期一

1.已对ICP匹配点对的过程进行加速（用kd-tree），加速后能达到760ms/帧（加速前是六七分钟每帧），用的测试数据集是每帧307200个点，也就是说没经过任何前期处理，如果加上类似下采样的前处理过程，匹配速度应该会再快一些。

2.之前用OpenCV中提供的计算三维仿射变换的函数来进行变换矩阵的估计，事实证明不太可行（计算出来的旋转矩阵似乎不是刚体变换中的那种旋转矩阵），具体原因我还没有找到，因为用OpenCV来进行这种处理的资料特别少所以我的用法是不是正确我也不是很确定。由于RANSAC的应用没有成功所以暂时没有能够给出ICP的初值，所以还没能评估ICP的准确性。

3.下一步先用RANSAC或其他办法完成粗配准，进而验证ICP匹配的效果；另外考虑如何从彩色图像上来获取信息（之前想利用提取到的特征点来进行粗配准，再与直接用RANSAC粗配准相比看对实时性是否有提高。

## 2013年11月6日星期三

1.修改后的程序与之前相比误差有所减小，如果不考虑非重合区域的影响，匹配点对之间的距离平均在5cm之内。

2.为了避免非重合区域的影响，我考虑在用Kdtree搜索最近点的过程中对距离进行一定的约束，应该可以在一定程度上消除非重合区域的影响。但这部分程序没有调出来，可能是哪里写的不对，在查。

3.进行三倍下采样后运行时间400ms（采样前4s），进行三次迭代过程。由于有非重合区域的影响（采到的数据集非重合区域较大），现在还无法确定一个很好的阈值来停止迭代，所以迭代次数暂时是直接给定的。

4.下一步争取实现对距离的约束来避免非重合区域影响，从而确定阈值再来检测匹配准确性；同时考虑如何利用彩色信息来实现初值的估计。

## 2013年11月7日

1.重新写了ICP实现的程序，之前的程序有点问题，导致收敛后的结果误差很大，由于没有实现用RANSAC估计初始变换，所以暂时用之前误差较大的结果来当做ICP的初始值。

2.用Kdtree加速了ICP中匹配点对的过程

|                                    | **每帧处理时间（迭代三次）** |
| :--------------------------------: | :--------------------------: |
|         ICP（未经下采样）          |     1082390ms  (~18min)      |
|         ICP（三倍下采样）          |           12913ms            |
| 用kd-tree加速后的ICP（未经下采样） |            4202ms            |
| 用Kd-tree加速后的ICP（三倍下采样） |            435ms             |

- Kd-tree：通过构建二分式的空间结构来完成邻近点的搜索，不用遍历被搜索空间的所有点，对所有点都计算距离，可以大幅减少处理时间。
- 三倍下采样后：对于kd-tree加速后的ICP，处理时间减少的程度不如原始ICP明显，因为每一次迭代都需要重新构建一遍kd-tree的空间结构。


3.多帧结果：10帧；处理时间统计如下表：

| 第n次匹配 | 读取数据（ms） | ICP匹配（ms） |
| :-------: | :------------: | :-----------: |
|     1     |      941       |      294      |
|     2     |      903       |      291      |
|     3     |      889       |      246      |
|     4     |      877       |      251      |
|     5     |      861       |      215      |
|     6     |      855       |      230      |
|     7     |      862       |      252      |
|     8     |      873       |      259      |
|     9     |      854       |      246      |
|   平均    |     879.4      |     253.8     |

- ICP已经过kd-tree加速，并三倍下采样，下采样并剔除无效点后平均每帧点数2.5万。
- 由于还没有实现初始值的估计，所以是采到10帧数据后，离线估计了两两之间的粗变换，再从文件中读取数据连续跑出来的结果。在线跑时初始值估计的时间应该会比读数据的时间要少，所以理论上在线总时间应该是小于现在统计的结果。

## 2013年11月18日星期一

1.进行ICP初始值的估计：为了用根据彩色图像提取到的特征点来完成粗配准，需要先完成特征点的匹配，得到匹配点对。之前用OpenCV中提供的FAST特征点提取算子实现特征点的提取，提取速度比Surf要快很多，但当把Surf生成描述符的方法应用于FAST提取到的特征点上时，得到匹配点对的匹配程度却很低。如果应用Surf算子来提取特征点的话匹配程度可以很高，但实时性无法保证（提取足够数量时平均一帧100ms）。

2.参考P.Henry的RGB-D mapping Using Kinect-style depth cameras for dense 3D modeling of indoor environments中提到，用FAST特征检测结合Calonder特征描述子可以得到更加快速和稳定的特征点。但我用Calonder描述子来处理FAST提取到的特征点时却无法得到特征描述符，还没找到什么问题。

3.下一步：明天再继续尝试下FAST+Calonder得到匹配点对。如果明晚之前还是行不通，就暂时先用Surf生成的匹配点对来进行变换矩阵的估计，把在线的实验跑出来之后，再回头做特征点匹配这一部分实时性的工作。

## 2013年11月19日星期二

1.已完成FAST+Brief（即Calonder）来提取特征并匹配，每帧提取到的特征点数量为160左右时，特征点提取时间4ms/两帧，描述符生成时间3ms/两帧，匹配时间1ms。之前没有成功的原因是，Brief特征描述符是以二进制串的形式来生成的，所以在匹配的时候应计算其汉明矩，我昨天在匹配那一步没有用到汉明矩的模板所以一直出错。匹配后的点投影到三维空间后如下图（已去除Kinect采集过程中的无效点）：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img1.jpg)

2.下一步：进行初始变换的估计。这几天仔细阅读了一下Horn在Closed-form solution of absolute orientation using unit quaternions中提出的用四元数来描述空间旋转的方法，可以通过两个点集中不共线的对应三点来计算出旋转和平移变换。考虑将这种方法应用于RANSAC中，一是去除掉错配的点对，二是得到一个变换关系，完成粗配准。

## 2013年11月25日星期一

1.目前进度：刚刚把RANSAC的结果跑出来。

粗匹配结果：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img2.png)

投影到x-z平面：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img3.png)

经过ICP：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img4.png)

投影到x-z平面：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img5.png)

程序现在改的有点乱，具体量化指标还没有计算，等这两天把程序先整合一下再一起计算。

2.大体统计了一下，所用的数据集每帧提取的特征点数大概在700到1500，最后筛选出的可用匹配点对大概在特征点数的五分之一到四分之一。因为特征点匹配的过程中存在大量错配点（如下图），而且由于特征点的位置一般都在物体边缘，所以在往三维空间投影时也会出现投影在三维空间后的特征点不再匹配的情况。

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img6.png)

3.下一步工作：（1）先把各个部分的程序整理一下，由于程序都是分开测试的，写的有点乱所以这次在传参的时候发生了一点小错误导致结果出不来，查了很久才查出来。（2）先跑一下多帧的情况，如果没问题应该就可以跑在线了。

## 2013年12月3日星期二

1.目前的进度：已运行过离线多帧，正在将Kinect在线采集的程序和离线处理的程序合成在一起。

2.下面是两个离线多帧匹配的结果：

结果一：10帧

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img7.png)

之前的实验结果表明，收敛后均方误差值若在30（mm）以内则看上去匹配效果已经比较好（因为在计算均方误差时，非重合区域的点在计算里也被考虑在内）。这个10帧数据集之前在采集的时候两帧之前的移动有点大，所以有些非重合区域较大的两帧之间在得到了较好的粗配结果之后，在运行ICP之后却偏离了原来的匹配位置。如下图所示。

粗配后：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img8.png)

ICP后：

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img9.png)

我又检验了其它的数据集，发现在非重合区域较小的两帧之间不会出现这种情况。所以我推测由于ICP是将全部点参与运算，所以只是将结果收敛到局部的最小值，并不能保证全局的最小。所以如果非重合区域的点数量过多的话可能会影响ICP的收敛结果（使其收敛到另一个局部最小值）。

关于这一点我之后再查查资料。

下表是每一帧最后收敛后的平均均方误差值的统计。

| 匹配帧 | 收敛后均方误差值(mm) |
| :----: | :------------------: |
|  1&2   |       27.0698        |
|  2&3   |       63.3997        |
|  3&4   |       24.3678        |
|  4&5   |       51.7308        |
|  5&6   |       61.6408        |
|  6&7   |       46.1378        |
|  7&8   |       29.0819        |
|  8&9   |        25.046        |
|  9&10  |       17.2721        |

结果二：5帧

![img](https://sunqinxuan.github.io/images/posts-research-journal-2013-11-img10.png)

| 匹配帧 | 收敛后均方误差值(mm) |
| :----: | :------------------: |
|  1&2   |       25.2674        |
|  2&3   |       37.6532        |
|  3&4   |       27.5926        |
|  4&5   |       32.0139        |

3.下一步工作：把采集与处理两部分程序合成一下，跑出在线结果；查一下关于ICP收敛后的局部最小值达不到期望的结果，是否与非重合区域有关。
