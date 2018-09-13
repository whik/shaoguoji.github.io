---
layout:          post
title:           CAN 诊断升级那点事儿（一） 
subtitle:        嵌入式软件的更新方法
date:            2018-08-29 22:26:39 +0800
author:          Shao Guoji
header-img:      
catalog:         true
tag:
    - 嵌入式
    - 汽车电子
---

### 关于本系列

入职不久后被安排编写产品的诊断升级上位机，于是疯狂啃各种协议、标准，车厂文档，加上自己的一些思考，也算粗糙做出来了。“项目驱动型”的学习加深了自己对 IAP 升级流程的理解，决定写点东西整理、分享当中的来龙去脉，便有了“CAN 诊断升级那点事儿”系列的 3 篇文章，包括：

* [](CAN 诊断升级那点事儿（一）—— 嵌入式软件的更新方法)：介绍嵌入式软件升级基本原理与知识
* [](CAN 诊断升级那点事儿（二）—— CAN 总线与 UDS 协议)：了解车载通讯协议与 UDS 基本服务（还没写~~~）
* [](CAN 诊断升级那点事儿（三）—— Flash BootLoader 流程)：结合实例分析软件烧写具体流程（还没写~~~）

文章主要内容围绕 CAN 诊断升级展开 —— 简单来说，是在 CAN 总线的基础上，基于 UDS 诊断协议，从 PC 端把固件数据传输给 ECU 并烧写到内部 Flash……说人话！！！好吧，其实就是给汽车的某个部件“刷机”。

#### 一些术语

* 上位机：给设备发送指令与数据的控制程序，通常运行在 PC 端，同时与单片机通讯。
* ECU(Electronic Control Unit)：电子控制单元。这里看作汽车上的 MCU（单片机）即可。
* CAN(Controller Area Network)：控制器局域网络。定义了网络通讯的物理层电气特性与链路层帧结构。
* UDS(Unified Diagnostic Services)：统一诊断服务。应用层协议，采用请求响应机制，可实现汽车诊断、数据传输功能。
* IAP(In Application Programming)：在应用编程。“编程”指的是程序数据烧录，可在程序运行期间完成此操作。代码分为 BootLoader 和 App 两部分。
* Flash：嵌入式设备的硬盘，程序安身之处。

*注：上述术语说明并不严谨，刻意侧重于有助理解文章的通俗解释。*

---

### 不安分的固件

在过去，嵌入式软件通常被固化存储器中，烧录后几乎不会改动，因此被称为固件（firmware）。但随着功能复杂程度的提升，固件往往需要频繁迭代更新。