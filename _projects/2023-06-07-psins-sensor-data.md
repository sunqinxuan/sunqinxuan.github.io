---
title: "PSINS-based Inertial Navigation System"
collection: projects
type: "Experimental Project"
excerpt: '...'
venue: "Inertial Navigation"
permalink: /projects/2023-06-07-ins
date: 2023-06-07
location: "Beijing, China"
---

The experiments used the open-source code [PSINS](http://www.psins.org.cn/sy) developed by [Dr. Yan Gongmin](https://teacher.nwpu.edu.cn/yangongmin.html) from Northwestern Polytechnical University.

## Experiment 1

### Experimental Setup

- Sensor: [WTGAHRS2](https://wit-motion.yuque.com/wumwnr/docs/rlp3gu)

- Handheld motion pattern.

- location: on the rooftop of China Satellite Communications Building, Zhichun Road, Haidian District, Beijing.

<!--手持传感器。

北京市海淀区知春路，中国卫星通信大厦楼顶。-->

### Experimental Results

| time(s)         | 0-850 (14min) | 850-1080 (18min) | 1080-1560 (26min) | 1560-1760 (29min) | 1760-1800 (30min) |
| ------------- | ------------- | ---------------- | ----------------- | ----------------- | ----------------- |
| motion pattern    | still            | still            | still            | moving               | figure-eight motion    |
| Attitude error - pitch (deg) | <4            | <10              | <19               | <50               | <50               |
| Attitude error - roll (deg) | <0.25         | <0.25            | <0.25             | <50               | <50               |
| Heading error (deg)     | <3            | <3               | <3                | <46               | <50               |

* Reference attitude/heading: AHRS, nominal accuracy: attitude 0.2 degrees, heading 1 degree.

### IMU 

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img1.png)

### Magnetometer

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img2.png)

### GPS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img3.png)

### INS 

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img4.png)

### AHRS & GPS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img5.png)

### Compare AHRS and INS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img6.png)


## Experiment 2

### Experimental Setup 

- Sensor: [WTGAHRS2](https://wit-motion.yuque.com/wumwnr/docs/rlp3gu)

- Vehicle-mounted motion.

- Region: one loop along Beijing Sixth Ring Road.

- Sensor installation (right-front-up, with the car facing the left side of the picture):

<!--
车载传感器，沿北京六环线一圈。

传感器安装（右-前-上，车头朝向图片左侧）：
-->

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img7.png)

### IMU

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img8.png)

### Mag

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img9.png)

### GNSS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img10.png)

<!--全程停留在 经度116.1deg，纬度39.8deg 附近，
并且，[16:02:53.683 - 16:03:07.057]时间区别内输出零值。-->
Remained near longitude 116.1 degrees and latitude 39.8 degrees throughout,
and output zero values during the time period [16:02:53.683 - 16:03:07.057].

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img11.png)

### INS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img12.png)

### Compare AHRS and INS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img13.png)


## Experiment 3

### Experimental Setup 

- Sensor: [3DM-GX5-GNSS/INS](https://www.microstrain.com/sites/default/files/3dm-gx5-45_datasheet_8400-0091_rev_o.pdf)

- Vehicle-mounted motion.

- Region: one loop along Beijing Sixth Ring Road.

- Sensor installation (front-right-down, with the car facing the left side of the picture):

<!--
车载传感器，沿北京六环线一圈。

传感器安装（前-右-下，车头朝向图片左侧）：
-->

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img14.jpg)

### IMU

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img15.png)

### Mag

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img16.png)

### GNSS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img17.png)

### INS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img18.png)

### Compare AHRS and INS

![img](http://sunqinxuan.github.io/images/projects-2023-06-07-img19.png)

- Attitude error <8.5deg
- Heading error
	- 0~1h: <10deg
	- 1~2h: <17deg
	- 2~4h: <50deg


## Related Links

code:
- [psins](https://github.com/sunqinxuan/psins)