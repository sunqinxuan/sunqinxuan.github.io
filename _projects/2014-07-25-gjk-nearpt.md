---
title: "Nearest Distance Calculation Based on GJK Algorithm"
collection: projects
type: "Research Project"
excerpt: '...'
venue: "Visual SLAM"
permalink: /projects/2014-07-25-gjk-nearpt
date: 2014-07-25
location: "Tianjin, China"
---

<!--基于GJK算法的物体最近距离计算方法-->

<!-- ## 基本算法原理

### 用GJK算法估计最近距离 

GJK（Gilbert-Johnson-Keerthi）算法是一种碰撞检测算法，用GJK算法来计算两个凸多边形之间的最短距离实际上就是它们之间的明可夫斯基差与原点的距离，若其明可夫斯基包含原点，则表示这两个凸多边形相交。

其中，明可夫斯基差实际上就是明可夫斯基和的概念。

假设有两个物体，他们的明可夫斯基和就是物体A上的所有点和物体B上的所有点的和集，即： -->

## Basic Algorithm Principles

### Estimating the Nearest Distance Using the GJK Algorithm

The GJK (Gilbert-Johnson-Keerthi) algorithm is a collision detection algorithm. Using the GJK algorithm to calculate the shortest distance between two convex polygons is essentially finding the distance from their Minkowski difference to the origin. If the Minkowski difference contains the origin, it indicates that the two convex polygons intersect.

The Minkowski difference is conceptually similar to the Minkowski sum.

Assume there are two objects. The Minkowski sum of these objects is the set of all sums of points from object A and points from object B, i.e.,

$$
A+B=\{a+b|a\in A,b\in B\}
$$

<!-- 显然如果两个物体都是凸体，则它们的明可夫斯基和也是凸体。而明可夫斯基差就是明可夫斯基和相对于减法的概念。可以表示为： -->
It is clear that if both objects are convex, their Minkowski sum is also convex. The Minkowski difference is the concept of the Minkowski sum with respect to subtraction and can be represented as:

$$
A-B=\{a-b|a\in A,b\in B\}
$$

<!-- 如果两个凸多边形没有碰撞，则其明可夫斯基差不包含原点，且此时它们之间的最短距离就是它们的明可夫斯基差形状到原点的最近距离。 -->
If the two convex polygons do not collide, their Minkowski difference does not contain the origin. In this case, the shortest distance between them is the nearest distance from the shape of their Minkowski difference to the origin.

<!-- ### Kd_tree搜索加速

Kd_tree（K_dimension tree）是对数据点在k维空间中划分的一种数据结构。Kd_tree是一种二叉树，每个节点表示的是一个空间范围。它是通过建立数据索引从而进行快速匹配的，因为实际的数据通常会呈现出簇状的聚类形态，通过设计有效的索引结构可以大大加快检索的速度。

在C++中，可以通过对ANN（A Library for Approximate Nearest Neighbor Searching）库函数的调用来直接实现Kd_tree的相关算法。ANN是一个用C++写成的库，支持各种高维最近邻搜索的数据结构和相关算法。 -->

### Accelerating Searches with Kd_tree

Kd_tree (K-dimensional tree) is a data structure used for partitioning data points in a k-dimensional space. A Kd_tree is a binary tree where each node represents a spatial range. It facilitates fast matching by creating data indexes, as real-world data often clusters together. By designing effective indexing structures, the search speed can be significantly improved.

In C++, the ANN (A Library for Approximate Nearest Neighbor Searching) library can be used to implement Kd_tree-related algorithms. ANN is a C++ library that supports various data structures and algorithms for high-dimensional nearest neighbor searches.

<!-- ### KNN算法

KNN（k_Nearest Neighbor）分类算法是最简单的机器学习算法之一。该算法是思路是，如果一个样本在特征空间中的k个最邻近的样本中的大多数属于某一个类别，则该样本也属于这个类别。KNN算法中，所选择的邻近样本都是已正确分类的对象。

KNN算法不仅可以用于分类，也可以用于回归。通过找出一个样本的k个最近邻，将这些近邻的属性平均值赋给该样本，就可以得到这个样本的属性。

该算法的一个主要不足是当样本平衡时，如一个类的样本容量远远大于其他类，有可能导致当输入一个新样本时，该样本的k个近邻中这个大容量的类样本会占多数。 -->

### KNN Algorithm

The KNN (k-Nearest Neighbor) classification algorithm is one of the simplest machine learning algorithms. The idea is that if a sample belongs to the same category as the majority of its k nearest neighbors in the feature space, then the sample is also classified into that category. In the KNN algorithm, the selected neighboring samples are correctly classified objects.

KNN can be used for both classification and regression. By finding the k nearest neighbors of a sample and assigning the average attribute value of these neighbors to the sample, the sample's attributes can be determined.

A major drawback of the KNN algorithm is when the samples are imbalanced. If the sample size of one class is much larger than others, a new sample's k nearest neighbors might be dominated by this large class, leading to biased classification.

<!-- ### CUDA原理

CUDA（Compute Unified Device Architecture）是显卡厂商NVIDIA推出的运算平台，是一种由NVIDIA推出的通用并行计算架构，可以使GPU能够解决复杂的计算问题。开发人员可以用C语言来为CUDA架构编写程序。

使用显示芯片来进行运算工作，和使用 CPU 相比，主要有几个好处：

1. 显示芯片通常具有更大的内存带宽。例如，NVIDIA 的 GeForce 8800GTX 具有超过 50GB/s 的内存带宽，而目前高阶 CPU 的内存带宽则在 10GB/s 左右。 

2. 显示芯片具有更大量的执行单元。例如 GeForce 8800GTX 具有 128 个 "stream processors"，频率为 1.35GHz。CPU 频率通常较高，但是执行单元的数目则要少得多。 

3. 和高阶 CPU 相比，显卡的价格较为低廉。例如目前一张 GeForce 8800GT 包括 512MB 内存的价格，和一颗 2.4GHz 四核心 CPU 的价格相若。

CUDA代码分为两部分，一部分是host（CPU）上运行，是普通的C代码；另一部分在device（GPU）上运行，是并行代码，称为kernel，由nvcc进行编译。Kernel产生的所有线程成为Grid。在并行部分结束后，程序回到串行部分即到host上运行。

在CUDA中，host和device有不同的内存空间。所以在device上执行kernel时，程序员需要把host memory上的数据传送到分配的device memory上。在device执行完以后，需要把结果从device传送回host，并释放device memory。CUDA runtime system提供了API给程序员做这些事情。 -->

### CUDA Principles

CUDA (Compute Unified Device Architecture) is a computing platform introduced by NVIDIA, allowing GPUs to handle complex computational problems. Developers can write programs for the CUDA architecture using the C language.

Using a GPU for computation offers several advantages over using a CPU:

1. GPUs generally have higher memory bandwidth. For example, the NVIDIA GeForce 8800GTX has a memory bandwidth exceeding 50GB/s, while high-end CPUs have memory bandwidths around 10GB/s.

2. GPUs have a larger number of execution units. For instance, the GeForce 8800GTX has 128 "stream processors" running at 1.35GHz. Although CPUs typically have higher frequencies, they have far fewer execution units.

3. Compared to high-end CPUs, GPUs are relatively inexpensive. For example, a GeForce 8800GT with 512MB of memory costs about the same as a 2.4GHz quad-core CPU.

CUDA code is divided into two parts: one part runs on the host (CPU) and is standard C code, while the other part runs on the device (GPU) and is parallel code, called a kernel, compiled by nvcc. All threads generated by the kernel form a Grid. After the parallel section ends, the program returns to the serial part and runs on the host.

In CUDA, the host and device have separate memory spaces. Therefore, when executing a kernel on the device, the programmer needs to transfer data from host memory to allocated device memory. After the device completes its execution, the results need to be transferred back to the host, and the device memory should be freed. The CUDA runtime system provides an API for programmers to perform these tasks.


<!-- ## 算法实现流程

### GJK算法的实现

流程如下图所示。 -->

## Algorithm Implementation Process

### Implementing the GJK Algorithm

The process is shown in the diagram below.

<img src="https://sunqinxuan.github.io/images/project-2014-07-25-img1.PNG" alt="GJK" style="zoom:100%;" />

<!-- ### 用ANN实现Kd_tree

假设物体A与B，对于物体B，开辟一片内存空间来存储B中的所有点，并定义一个ANNpointArray类的指针指向这片内存空间，并用物体B中的点来构建Kd_tree结构，存储在ANNkd_tree类中。对于物体A中的每一个点，都赋给ANNpoint类作为一个查询点，通过ANNkd_tree类中的annkSearch函数来对物体B中的点进行搜索，找出一个最近邻点作为物体A中某点的最近点。

找到最近点后用欧式距离的计算公式直接来进行距离的计算，然后通过物体A中每个点对应距离的依次比较来确定最终物体A和B的最近点。 -->

### Implementing Kd_tree with ANN

Assume there are objects A and B. For object B, allocate a memory space to store all points in B and define a pointer of the ANNpointArray class to point to this memory space. Build a Kd_tree structure using the points from object B, and store it in the ANNkd_tree class. For each point in object A, assign it to the ANNpoint class as a query point. Use the annkSearch function in the ANNkd_tree class to search for points in object B and find the nearest neighbor as the closest point to a given point in object A.

Once the nearest point is found, calculate the distance directly using the Euclidean distance formula. Then, compare the distances corresponding to each point in object A to determine the closest point between objects A and B.

<!-- ### 点到面距离的实现

采样较为稀疏的物体，在Kd_tree算法的基础上得到的最近点可能与两个物体实际上的最近点相差较远。但对于表面情况比较单一的物体，太密集的采样不但影响效率，而且信息量不大。所以针对这样的情况，提出利用点到面距离在一个三角形确定到三角形外某一点的最近点。

具体实现方法：

假设两个物体A和B，用Kd_tree的算法得到了最近点a和b，在物体B上确定点b所在的三角形（可能不止一个），对于每个三角形找到距离a最近的点，再经过依次的比较得到最近距离对应的那个点即为两个物体的最近点。

如下图所示，茶壶为物体A，圆柱体为物体B，而物体B只有上下底面的圆周才有采样点。 -->

### Implementing Point-to-Face Distance

For sparsely sampled objects, the closest points obtained using the Kd_tree algorithm may differ significantly from the actual closest points between the two objects. However, for objects with relatively uniform surfaces, dense sampling can be inefficient and uninformative. In such cases, using point-to-face distance to determine the nearest point outside a triangle is proposed.

Implementation method:

Assume objects A and B. Use the Kd_tree algorithm to find the nearest points a and b. Identify the triangle on object B where point b lies (there may be more than one). For each triangle, find the point closest to a, and through successive comparisons, determine the point corresponding to the shortest distance as the nearest point between the two objects.

As shown in the diagram below, the teapot is object A, and the cylinder is object B, with sampling points only on the top and bottom circumferences of object B.

![img](https://sunqinxuan.github.io/images/project-2014-07-25-img2.PNG)

<!-- ## 算法测试

### GJK

实现速度：~90fps（两个物体的采样点数分别为10000个点和2500个点）。

碰撞检测实现的效果较稳定。

距离的计算也比较准确。

存在问题：

- 最近点的确定。因为GJK算法本身不涉及最近点的确定，它正是由于通过迭代算法避开了对点的遍历，才能获得很高的速度，所以无法从算法本身来确定最近点，而需要运用其它的方法。

- 由于GJK算法只适用于凸多边形或凸多面体，所以在实际应用背景下，可能多数情况都不能直接运行该算法来进行距离的计算，从而涉及到如何将复杂物体分割成多个凸多面体，而这个分割的过程是否能满足实时性和准确性的要求。 -->

## Algorithm Testing

### GJK

Implementation speed: ~90 fps (sampling points of 10,000 and 2,500 for the two objects, respectively).

Collision detection performance is quite stable.

Distance calculation is also fairly accurate.

Issues:

- Determining the nearest point: The GJK algorithm itself does not involve determining the nearest point. It achieves high speed by avoiding point traversal through iterative algorithms. Therefore, it cannot determine the nearest point intrinsically and requires other methods to do so.

- The GJK algorithm is only suitable for convex polygons or convex polyhedra. In practical applications, it may not be directly usable for most cases, which raises the issue of how to decompose complex objects into multiple convex polyhedra. The decomposition process must meet real-time and accuracy requirements.

<!-- ### ANN

实现速度：~30fps（两个物体的采样点数都为5000个点）。

最近点搜索的结果较稳定，且因其只是针对点的搜索过程，所以适应性较强，对物体本身的形状没有特殊要求。

存在问题：

- 由于距离是直接用得到的最近点来计算的，所以对于采样点较稀疏的物体来说，得到的最近距离可能并不是两个物体之间的最近距离。 -->

### ANN

Implementation speed: ~30 fps (sampling points of 5,000 for each object).

The nearest point search results are quite stable. Since it only involves searching for points, it has strong adaptability and no special requirements for the shape of the objects.

Issues:

- Since the distance is calculated directly from the nearest point obtained, for sparsely sampled objects, the nearest distance found may not be the actual closest distance between the two objects.

![img](https://sunqinxuan.github.io/images/project-2014-07-25-img3.gif)

## related links

code:
- [gjk_nearpt](https://github.com/sunqinxuan/gjk_nearpt)

patent:
- [CN104462764A](http://sunqinxuan.github.io/files/publications-2015-03-25-patent-CN104462764A.pdf)