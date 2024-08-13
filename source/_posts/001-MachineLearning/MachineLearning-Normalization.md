---
title: MachineLearning-Normalization
date: 2022-08-25 22:24:38
updated: 2022-08-25 22:24:38
tags: A First Course in ML
categories: MachineLearning
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
mathjax: true
---

# ***MachineLearning-Normalization***



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
>3. [预测](#预测 "预测")
>8. [正则化最小二乘法](#正则化最小二乘法 "正则化最小二乘法")
> 9. [后续章节内容预告](#后续章节内容预告 "后续章节内容预告")
>10. [引用文献](#引用文献 "引用文献")#下载图片接口 "下载图片接口")



### 概述

为什么需要Normalization？

深度学习模型训练的难度是，CNN包含了很多不同的hiden layer， 每个layer都会随着训练改变，hiden layer的输入分布也是会一致改变，这就导致hiden layer面临covariate shift的问题。Internal covariate shift(ICS)使得每层输入不再是独立的分布，上一层数据需要适应新的输入分布，数据输入激活函数时，会落入饱和区，使得学习效率过低，甚至梯度消失。

Normlization的思想主要是通过归一化的手段，将每层的输入强行拉回到均值为0，方差为1的标准正态分布，使得激活输入值分布在非线性函数梯度敏感的区域内，避免梯度消失，加快训练速度。



比如将sigmoid函数，BN将输入值分布在-1～1的区间内，这个区间的梯度值大，可以避免梯度消失，提高收敛的速度。

归一化后，激活输入值分布在[-1,1]内，这导致非线性程度降低，这样本来需要非线性函数进行learning高维度的表达的效果就降低了，所以BN就是为了保证非线性的表达能力，对变换后的输入$X$进行了平移。使用scale shift操作，$y=scale * x + shift$， 这两个参数通过训练获得。

均值的计算和方差的计算



参考https://blog.csdn.net/litt1e/article/details/105817224




***

### ***后续章节内容预告***：

***

### 引用文献：
