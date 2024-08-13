---
title: TensorRT概述
date: 2022-08-24 23:47:00
updated: 2022-08-24 23:47:00
tags: TensorRT
categories: 
- NVIDIA
- TensorRT
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


## ***TensorRT概述***

TensorRT是一个NVIDIA给自己的显卡开发的一个对于深度学习模型进行加速推理及优化，以满足部署需求的C++ SDK（Software Development Kits), 常常和[ONNX]()（Open Neural Network Exchange）通用模型联系在一起。

### ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，同时转载请说明出处，感谢！

---

### 大纲：

>1. [概述](#概述 "概述")

## 概述

### TensorRT介绍

NVIDIA TensorRT是一个用来优化训练后的深度学习模型的SDK，这个SDK可以在模型推理的时候提高性能。TensorRT包含了一个深度学习的推理优化器（优化训练好的模型）和一个执行器（部署推理）。使用TensoRT可以使你的模型运行时有更高的吞吐量和更低的延迟。

**如图所示**：  

<img alt="01-TensorRT概述-b0cc304a.png" src="01-TensorRT概述-b0cc304a.png" width="" height="" >


***

### ***后续章节内容预告***：

***

### 引用文献：
