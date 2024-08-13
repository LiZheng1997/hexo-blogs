---
title: NVIDIA-Container-Toolkit
date: 2022-08-25 12:33:37
updated: 2022-08-25 12:33:37
tags: [Container-Toolkit, Docker]
categories: 
- NVIDIA
- Container-Toolkit
keywords: 
description:
top_img:
comments: true
cover:
toc: true
toc_number: true
copyright_author: Bluet
copyright_author_href:
copyright_url:
copyright_info:
mathjax: 
---

# ***NVIDIA-Container-Toolkit笔记整合***

NVIDIA Container Toolkit是支持多种Linux系统发行版本和不同容器引擎的一个工具包。

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，同时转载请说明出处，感谢！

---

## 大纲：

>1. [概述](#概述 "概述")
>2. [线性建模](#线性建模 "线性建模")
>     2.1. [什么是线性模型](#什么是线性模型 "什么是线性模型")
>     2.2. [什么是损失函数](#什么是损失函数 "什么是损失函数")
>     2.3. [对损失函数求偏导](#对损失函数求偏导 "对损失函数求偏导")
>     2.4. [二阶导数的意义图解](#二阶导数的意义图解 "二阶导数的意义图解")
>     2.5. [$w_0$和$w_1$的二阶导数](#$w_0$和$w_1$的二阶导数 "$w_0$和$w_1$的二阶导数")
>3. [后续章节内容预告](#后续章节内容预告 "后续章节内容预告")
>4. [引用文献](#引用文献 "引用文献")

[TOC]



### 概述

参考官方的解释，NVIDIA Container Toolkit（以下简称NCT）允许用户来建立和运行GPU加速的容器。这个工具包包含了一个容器运行时库（runtime library)和一些共用组件来自动化配置容器，以使得容器可以利用主机的GPU，如图所示的架构：

![5b208976-b632-11e5-8406-38d379ec46aa](https://www.synotech.top:5523/uploads/2023/04/23/202304231942295.png)

NCT同时也支持不同的容器引擎[Docker](https://docs.docker.com/get-started/overview/), [LXC](https://linuxcontainers.org/), [Podman](http://podman.io/) etc.

### 架构图

在使用nvidia-docker wrapper的时候，整个组件的流程图如下所示：

![nvidia-docker-arch-new](https://www.synotech.top:5523/uploads/2023/04/29/202304291433275.png)



***

### 引用文献：
