# 简介

**Author**：Iccccy     **Data**：2022-02-08

> 参考资料：
>
> [Webots: robot simulator (cyberbotics.com)](https://www.cyberbotics.com/)
>
> [到底应该使用哪款仿真器来仿真我的机器人 - 古月居 (guyuehome.com)](https://www.guyuehome.com/8218)

## 官方概述

Webots是一个开源和多平台的桌面应用程序，用于模拟机器人。它提供了一个完整的开发环境来建模、编程和模拟机器人。

它是为专业用途而设计的，并广泛用于工业、教育和研究领域。Cyberbotics Ltd.自1998年以来一直将Webots作为其主要产品。

<iframe width="874" height="491" src="https://www.youtube.com/embed/O7U3sX_ubGc?list=TLGGtEGuO6liMoAwODAyMjAyMg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

（视频来源：YouTube）

## 主要特点

| 描述     | 特点              | 备注                                                         |
| -------- | ----------------- | ------------------------------------------------------------ |
| 物理引擎 | 基于改进的ODE     | ODE：开源物理引擎，全称Open Dynamics Engine，<br />它是一款模拟刚体动力学的基于C/C++高性能库，功能稳定，<br />常被用于计算机游戏和虚拟现实等技术上； |
| 支持平台 | 全平台支持        |                                                              |
| 传感类型 | 十分丰富          | 传感器类型相当丰富，有多种传感器可供用户选择，<br />美中不足的是官方给定的力传感器只支持三轴力的检测而不支持三轴力矩的检测； |
| 编程接口 | 内置/自定义控制器 | 支持C/C++、Java、Python、MATLAB、ROS以及TCP/IP完成控制器编程，<br />不同的编程语言需要查阅不同的API，函数命名接近但风格不同，<br />ROS通信是通过调用ROS API，然后将所使用的语言对应的控制器代码<br />封装成ROS topic的形式来完成，自定义ROS控制器时建议使用Python；<br />R2020版本已经大大简化了ROS的使用过程； |
| 上手难度 | 最为简单          | 图形化界面、移动机器人仿真最简单且易用；                     |

