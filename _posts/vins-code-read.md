---
title: vins-mono 代码阅读笔记
date: 2018-02-03 12:41:46
tags: 
- slam 
- vins 
categories: 
- 机器人视觉
---

## 关键分析
1. **Ceres的使用**
    > - [博客：Ceres Solver的使用和Bundle Adjustment](http://jinjaysnow.github.io/blog/2016-12/Ceres%E4%BD%BF%E7%94%A8.html)
1. **SLAM中的Sliding Window和Marginalization**
    > - [SLAM中的marginalization 和 Schur complement](http://blog.csdn.net/heyijia0327/article/details/52822104)
    > - [泡泡机器人SLAM原创专栏-滑动窗算法](http://diyitui.com/mip-1479517546.62857641.html)
    > - [DSO 中的Windowed Optimization](http://blog.csdn.net/heyijia0327/article/details/53707261)

## 疑惑
1. **IMU Pre-intergration Jacob矩阵推导**
    >论文讲的都是用简单的Euler method算IMU积分的情况， 利用状态对时间的导数，把状态转换函数用此导数一阶线性近似。 jacob和covariance的推导都比较简单。但实际代码中用的是midpoint-method, 不容易把状态对时间求导， 所以直接转换方程对上一时刻状态求导， jacob很容易推导出来，但covariance涉及到非线性的状态变换，求Covariance比较复杂
    > 1. 递归求Jacob的推导:
    >> - [Quaternion Kinematics for the Error-State Kalman filte P77]()
    >> - [VINS-Mono: A Robust and Versatile Monocular Visual-Inertial State Estimator P3]()
    > 2. 非线性状态变换，covariance的推导
    >> - [State Estimation for Robotics P41:General Case via Linearization]()
。

    > 这里的MidPoint的jacob的计算涉及到扰动模型，具体参考以下文档
    >> - [GitHub Vins-Mono Issue: MidPoint Intergration](https://github.com/HKUST-Aerial-Robotics/VINS-Mono/issues/14)
    >> - [Quaternion kinematics for the error-state Kalman filte P43,P46,P51]()
    >> - [Indirect Kalman filter for 3D Attitude Estimation:A Tutorial for Quaternion Algebra P9]()
    >> - Quaternion_A*Vector3_B 对A的求导法则
    >> - Quaternion_A*Quaternion_B 对A和对B的求导法则

2. **Gyr Bias 计算**
    > 文件：initial_aligment.cpp， 这里为何要减去一个vi*t?, 原因是IMU  计算位置差差时没有考虑初速度的，所以这里要剪掉初速度对位置的影响
    
    >![](http://ortmp0r6f.bkt.clouddn.com/201707051007_234.png)

    >![](http://ortmp0r6f.bkt.clouddn.com/201707051013_156.png)
    
    >![](http://ortmp0r6f.bkt.clouddn.com/201707051015_941.png)
3. **边际化优化***
4. **ceres优化**
