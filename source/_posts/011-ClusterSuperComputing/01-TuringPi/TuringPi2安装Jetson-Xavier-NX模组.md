---
title: TuringPi2安装Jetson-Xavier-NX模组
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-06 10:44:50
updated: 2023-06-06 10:44:50
tags: [TuringPi2, Jetson]
categories: 
- 集群超算
- TuringPi
keywords: TuringPi2 Jetson-Xavier
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***TuringPi2安装Jetson-Xavier-NX模组***

TuringPi2安装Xavier-NX模组的个人经验描述，不具有官方的正确性，但是可以完成目标。

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请注明出处，感谢！

---

# 安装注意事项：

1. 目前在我尝试了多次直接在板载上刷写系统的情况下，并无法直接通过Turing Pi2上的USB2.0的口成功完成系统刷写后的**启动操作**。按我的理解，因为Xavier刷写系统完成之后，启动的时候，USB2.0就不能一直是Device模式，需要切换成Host模式。

1. 目前我将Xavier NX模组安装在Node1的位置，因为只有Node1可以连接HDMI的输出接口，因为刷写系统后，我需要可视化界面安装和操作一些指令。

1. 由于USB2.0的接口只能用在系统刷写，我们无法再Node1上使用无线键鼠，所以我们需要提前将一些必要的软件装上，比如Nomachine\Todesk之类的远程桌面软件，这样可以忽略显示器输出和键鼠的问题。



# 安装前的工作

1. 安装M.2硬盘

如果需要使用M.2的硬盘，在刷写系统之前，需要安装好硬盘，然后将系统刷写在NVME的硬盘上，再SDKManager上有nvme的选项，这样系统将会刷写在固态硬盘上，启动的时候从固态硬盘启动。由于Xavier NX开发者套件的模组是没有EMMC的，必须要插上一个SD卡才能正常刷写系统，所以需要安装一个SD卡。同时由于Xavier NX在SD卡上刷写系统后，性能以及存储空间不够的问题，我们优先选择将系统刷写在NVME的固态硬盘上，这样系统性能以及存储空间都满足后期的要求。

**开发者套件底板**

<img src="https://www.synotech.top:5523/uploads/2023/06/06/202306061532390.jpg" alt="42c00ea160cf31f1475bc223de92796" style="zoom:50%;" />



2. 刷写Jetpack系统

由于以上解释的问题和原因，我们需要提前将Xavier NX模组进行系统刷写和必要软件安装，我这里采用Deveploer Kit的底板安装好模组之后，直接通过SDKManager进行系统刷写和Jetson相关的软件安装。

**SDKManager刷系统界面**

![1685984312(1)](https://www.synotech.top:5523/uploads/2023/06/06/202306061540638.jpg)

# 安装Xavier-NX模组

## 选择合适的Node位置

在Turing Pi2上安装Xavier模组，需要提前考虑后期的使用场景中，需要用到哪些常用的接口，比如USB3.0、M.2、HDMI等，这样根据不同的Node功能接口特点，选择你需要安装的位置。

我这里选择放在Node1的位置，主要是我需要Xavier-NX后期做一些显示输出的工作，比如仿真模型、UI界面等，Node1接口包含了一个HDMI接口。

### **安装所选的Node位置示意图**

<img src="https://www.synotech.top:5523/uploads/2023/06/06/202306061540863.jpg" alt="bdecb376f540886ba8e5e9cd4c08fc6" style="zoom:50%;" />

## 安装硬件及硬盘

因为我们使用了NVME固态硬盘，我们需要根据选择Node位置，安装在对应的背面上SLOT的位置，Node1对应Slot1，以此类推，所以我这里安装在Slot1上，这样我们启动之后，Node1位置上安装的Xavier模组才能够从M.2固态硬盘启动。

### **硬盘对应Node安装示意图**

![803586cd0d24d4c59c14d926f528e7e](https://www.synotech.top:5523/uploads/2023/06/06/202306061541481.jpg)

## Xavier模组的散热排线

一定要将Xavier模组的散热排线接口接在Turing Pi2上的对应接口，这个供电接口在Node的插槽位置旁边，4个针脚的白色方形口。排线延长线默认发货的时候提供了，因为Xavier模组自带的风扇供电线长度比较短，一般需要使用到这个供电延长线。

### **散热延长排线安装示意图**

![336458f3eb3b94829121d391352a306](https://www.synotech.top:5523/uploads/2023/06/06/202306061541510.jpg)

***

### 引用文献：
