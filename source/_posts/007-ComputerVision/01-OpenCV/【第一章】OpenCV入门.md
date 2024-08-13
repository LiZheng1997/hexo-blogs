---
title: 【第一章】OpenCV入门
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax:
date: 2023-03-23 18:11:03
updated: 2023-03-23 18:11:03
tags: OpenCV
categories: 
- ComputerVision
- OpenCV
keywords:
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# 【第一章】OpenCV入门



## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，同时转载请说明出处，感谢！

---

## 大纲：

>1. [概述](#概述 "概述")
>2. [环境配置](#环境配置 "环境配置")
> 3. [图像处理基础操作](#图像处理基础操作 "图像处理基础操作")
> 4. [OpenCV贡献库](#OpenCV贡献库 "OpenCV贡献库")
> 5. [后续章节内容预告](#后续章节内容预告 "后续章节内容预告")
> 6. [引用文献](#引用文献 "引用文献")



# 概述

OpenCV是一个开源的计算机视觉库，1999年由英特尔的Gary Bradski启动。同时，OpenCV库由C和C++语言编写，涵盖计算机视觉各个领域内的500多个函数，可以在多种操作系统上运行。它旨在提供一个简洁而又高效的接口，从而帮助开发人员快速地构建视觉应用。



# 环境配置

- Python环境，Python3.x

- Ubuntu操作系统、Windows系统、macOS系统均可

- Anaconda安装配置　[__下载链接__](https://www.anaconda.com/download/)（根据自己的系统环境进行选择）

- 使用Pip进行OpenCV库的安装，当前仓库代码使用OpenCV3.4.3.18

```text
pip install opencv-python
```

以上环境配置好后，就可以开始练习OpenCV的库函数使用。



**注意：**本章节源码仓库可以从链接中自由获取，同时，本章节也有Jupyter Notebook的实现也在仓库中。Jupyter Notebook的使用和安装参考博客：[__https://medium.com/python4u/jupyter-notebook%E5%AE%8C%E6%95%B4%E4%BB%8B%E7%B4%B9%E5%8F%8A%E5%AE%89%E8%A3%9D%E8%AA%AA%E6%98%8E-b8fcadba15f__](https://medium.com/python4u/jupyter-notebook%E5%AE%8C%E6%95%B4%E4%BB%8B%E7%B4%B9%E5%8F%8A%E5%AE%89%E8%A3%9D%E8%AA%AA%E6%98%8E-b8fcadba15f)



# 图像处理基础操作

1. 读取图像

1. 显示图像

1. 使用waitKey函数

1. waitKey函数实现程序暂停

1. 销毁窗口

1. 销毁所有的窗口

1. 保存图像



# OpenCV贡献库

除了OpenCV的主库函数以外由OpenCV团队维护，OpenCV还有一个开源社区开发维护的贡献库，包含的视觉应用比OpenCV库更全面。

包含的一部分扩展模块有如下：

● bioinspired：生物视觉模块。

● datasets：数据集读取模块。

● dnn：深度神经网络模块。

● face：人脸识别模块。

● matlab:MATLAB接口模块。

● stereo：双目立体匹配模块。

● text：视觉文本匹配模块。

● tracking：基于视觉的目标跟踪模块。

● ximgpro：图像处理扩展模块。

● xobjdetect：增强2D目标检测模块。

● xphoto：计算摄影扩展模块。

**安装指令：**

```text
pip install opencv-contrib-python
```

**FAQ：**[__https://pypi.org/project/opencv-contrib-python/__](https://pypi.org/project/opencv-contrib-python/)




***

### ***后续章节内容预告***：

***

### 引用文献：

OpenCV轻松入门：面向Python/李立宗著.—北京：电子工业出版社，2019.5
