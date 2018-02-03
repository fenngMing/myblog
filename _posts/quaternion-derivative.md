---
title: slam中的四元数求导
date: 2018-02-03 21:58:30
tags: 
- slam 
- vins 
- math
categories: 
- 机器人视觉
---

> 四元数在机器人控制&机器人视觉领域被广泛运用，那么如何对不满足欧式空间的四元数（四个变量只有三个自由度）求导？
<!-- more -->

### 四元数$\mathbf{q}$乘以三维向量$\mathbf{v}$对四元数求导$\frac{\mathrm{\partial}\mathbf{q \cdot v}}{\mathrm{\partial}\mathbf{q}}$




### 资料
- [Tightly-coupled Visual-Inertial Sensor Fusion based on IMU Pre-Integration/Analytic Jacobian
A.3.1]()
> **对陀螺仪bias求导可以参考这儿**
![](http://ortmp0r6f.bkt.clouddn.com/201711291956_687.png)

- [Quaternion Kinematics for the Error-State KF/Useful, and very useful, Jacobians of the rotation]
> **四元数乘以三维向量，对四元数的vector部分求导可以参考这儿**
![](http://ortmp0r6f.bkt.clouddn.com/201711291958_377.png)

> [Indirect Kalman filter for 3D Attitude Estimation/P9]()
> **四元数乘以三维向量，对四元数的vector部分求导可以参考这儿**
![](http://ortmp0r6f.bkt.clouddn.com/201711292041_661.png)

![](http://ortmp0r6f.bkt.clouddn.com/201711292045_376.png)


