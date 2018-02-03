---
title: 关于Imu Gyro读数的理解
date: 2018-02-03 20:22:46
tags: 
- slam 
- vins 
categories: 
- 机器人视觉
---

> 本文主要介绍Imu Gyro读数（表示角速度）的含义和背后的数学推导
<!-- more -->

### Imu Gyro的输出欧拉角速度
陀螺仪的输出为x,y,z轴此刻的角速度读数{% raw %}$\vec{\omega}${% endraw %}, 经过$\Delta t$后三轴的旋转为欧拉角$\vec{\theta}$:
{% raw %}
$$
\vec{\omega} = \left[\begin{matrix} \omega_x\\\omega_y\\\omega_z \end{matrix}\right]

\vec{\theta} = \vec{\omega} \cdot \Delta t  = \left[\begin{matrix} \theta_x\\\theta_y\\\theta_z \end{matrix}\right]
$$
{% endraw %}


### 旋转四元数:

绕$\vec{\mathbf{k}}$轴旋转$\theta$角的四元数表示为：
{% raw %}
$$
\mathbf{q} = \left[\begin{matrix}
\cos \left( \theta/2 \right) \\
\vec{\mathbf{k}} \cdot \sin \left( \theta/2 \right)
\end{matrix}\right]
$$
{% endraw %}
当$\Delta t \rightarrow 0$时, $\theta \rightarrow 0$，此时我们可以用一阶Taylor展开近似表示$\mathbf{q}$:
{% raw %}
$$
\mathbf{q} = \left[\begin{matrix}
\cos \left( \theta/2 \right) \\ 
\vec{\mathbf{k}} \cdot \sin \left( \theta/2 \right)
\end{matrix}\right]  
\approx 
\left[\begin{matrix}
1\\
\vec{\mathbf{k}}\cdot \theta/2
\end{matrix}\right]
=
\left[\begin{matrix}
1\\
\frac{1}{2} \cdot \vec{\theta}
\end{matrix}\right]
$$
{% endraw %}
此时，若$\vec{\theta}$对时间求导，也会求得一个$\vec{\omega}$。 
{% raw %}
$$
\vec{\omega} = 
\lim\limits_{\Delta t \rightarrow 0}\frac{\vec{\theta}}{\Delta t }
$$
{% endraw %}

### 当旋转非常小时，欧拉角到四元数的关系:
定义$\psi,\theta,\phi$为绕Z轴、Y轴、X轴的旋转角度（即Yaw、Pitch、Roll），则欧拉角的定义为：
{% raw %}
$$
\mathbf{r} =  \left[\begin{matrix}
\phi \\
\theta 
\\\omega
\end{matrix}\right]
$$
{% endraw %}
欧拉角到四元数的转换关系为：

![](http://ortmp0r6f.bkt.clouddn.com/201711302016_223.png)

当$\psi,\theta,\omega$都非常小时, 用一阶Taylor展开四元数中各个sin项，然后抛弃掉高阶无穷小项可得：
{% raw %}
$$
\mathbf{q} = \left[\begin{matrix} w \\ x \\ y \\ z \end{matrix}\right] =
\left[\begin{matrix} 
1 \cdot 1 \cdot 1 + \frac{\phi}{2} \cdot \frac{\theta}{2} \cdot \frac{\omega}{2} \\
\frac{\phi}{2} \cdot 1 \cdot 1 - 1 \cdot \frac{\theta}{2} \cdot \frac{\omega}{2} \\
1 \cdot \frac{\theta}{2} \cdot 1  +  \frac{\phi}{2} \cdot 1 \cdot  \frac{\omega}{2} \\
1 \cdot 1 \cdot \frac{\omega}{2} - \frac{\phi}{2} \cdot \frac{\theta}{2} \cdot 1
 \end{matrix}\right] = 
\left[\begin{matrix} 
1 + \frac{\phi \cdot \theta \cdot \omega}{8} \\
\frac{\phi}{2} - \frac{\theta \cdot \omega }{4} \\
\frac{\theta}{2} - \frac{\omega \cdot \phi }{4} \\
\frac{\omega}{2} - \frac{\phi \cdot \theta }{4} \\
\end{matrix}\right] = 
\left[\begin{matrix} 
1 \\
\frac{1}{2} \cdot \mathbf{r}
\end{matrix}\right]
$$
{% endraw %}

## 由上可以直接得到gyr读数到旋转四元数的转换：
{% raw %}
$$
\mathbf{q} = 
\left[\begin{matrix}
1\\
\frac{1}{2} \cdot \vec{\omega} \cdot \Delta t
\end{matrix}\right]
=
\left[\begin{matrix}
1\\
\frac{1}{2} \cdot \vec{\theta}
\end{matrix}\right]
$$
{% endraw %}

### Vins-moblie 代码实现:
![](http://ortmp0r6f.bkt.clouddn.com/201711291711_553.png)


### 参考资料：
- [Indirect Kalman Filter for 3D Attitude Estimation/Quaternion Time Derivative]()
- [Quaternion Kinematics for the Error-State KF/Quaternion and rotation vector]()

