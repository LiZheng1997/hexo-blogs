---
title: Pytorch介绍
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-09-01 10:37:37
updated: 2023-09-01 10:37:37
tags: Pytorch
categories:
- DeepLearning
- Pytorch
keywords:
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***Pytorch介绍***



## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请说明出处引用，感谢！

---



# Torch的背景

Torch是一个与Numpy类似的张量（Tensor）操作库，与Numpy不同的是Torch对GPU支持的很好，Lua是Torch的上层包装。



# PyTorch

PyTorch是一个基于Torch的Python开源机器学习库，它主要由Facebook的人工智能研究小组开发。Uber的"Pyro"也是使用的这个库。

PyTorch是一个Python包，提供两个高级功能：

- 具有强大的GPU加速的张量计算（如NumPy）

- 包含自动求导系统的的深度神经网络



# Tensorflow VS Pytorch

## 上手时间

PyTorch本质上是Numpy的替代者，而且支持GPU、带有高级功能，可以用来搭建和训练深度神经网络。如果你熟悉Numpy、Python以及常见的深度学习概念（卷积层、循环层、SGD等），会非常容易上手PyTorch。

TensorFlow可以看成是一个嵌入Python的编程语言。你写的TensorFlow代码会被Python编译成一张图，然后由TensorFlow执行引擎运行。我见过好多新手，因为这个增加的间接层而困扰。也正是因为同样的原因，TensorFlow有一些额外的概念需要学习，例如会话、图、变量作用域（variable scoping）、占位符等。



## **图创建和调试**

Pytorch的图结构是动态的，在运行时构建。Tensorflow图结构是静态的。

调试的时候，Pytorch可以像调试python代码一样，使用pdb并在任何地方可以设置断点。调试Tensorflow需要从会话请求中检查变量的情况，或者学会使用Tensorflow的调试器tfdbg。

![](https://www.synotech.top:5523/uploads/2023/09/01/202309011045395 "")

## 全面性

Tensorflow支持的功能比Pytorch更多，截止写博客的时间，这个差异很小了。Tensorflow有的更多功能点：

- 沿维翻转张量（np.flip, np.flipud, np.fliplr）

- 检查无穷与非数值张量（np.is_nan, np.is_inf）

- 快速傅里叶变换（np.fft）

## 序列化

两种框架下保存和加载模型都很简单。PyTorch有一个特别简单的API，**可以保存模型的所有权重或pickle整个类**。TensorFlow的Saver对象也很易用，而且为检查提供了更多的选项。

TensorFlow序列化的主要**优点是可以将整个图保存为protocol buffer**。包括参数和操作。然而图还能被**加载进其他支持的语言**（**C++、Java**）。这对于**部署堆栈至关重要**。理论上，当你想改动模型源代码但仍希望运行旧模型时非常有用。

## 部署

对于小规模的服务器端部署（例如一个Flask web server），两个框架都很简单。

对于移动端和嵌入式部署，TensorFlow更好。不只是比PyTorch好，比大多数深度学习框架都要好。使用TensorFlow，部署在Android或iOS平台时只需要很小的工作量，**至少不必用Java或者C++重写模型的推断部分**。

对于高性能服务器端的部署，还有TensorFlow Serving能用。除了性能之外，**TensorFlow Serving一个显著的优点是可以轻松的热插拔模型，而不会使服务失效**。

## 数据加载

PyTorch中用于加载数据的API设计的很棒。接口由**一个数据集、一个取样器和一个数据加载器构成**。数据加载器根据取样器的计划，基于数据集产生一个迭代器。**并行化数据加载简单的就像把num_workers参数传递给数据加载器一样简单。**

TensorFlow中没有发现特别有用的数据加载工具。很多时候，并不总能直接把准备并行运行的预处理代码加入TensorFlow图。以及API本身冗长难学。

## **设备管理**

TensorFlow的**设备管理非常好用**。通常你不需要进行调整，因为**默认的设置就很好**。例如，TensorFlow会假设你想运行在GPU上（如果有的话）。而在PyTorch中，即使启用了CUDA，你**也需要明确把一切移入设备**。

TensorFlow设备管理唯一的缺点是，默认情况下，它会占用所有的GPU显存。简单的解决办法是**指定CUDA_VISIBLE_DEVICES**。有时候大家会忘了这一点，所以GPU在空闲的时候，也会显得很忙。

在PyTorch中，我发现**代码需要更频繁的检查CUDA是否可用**，以及**更明确的设备管理**。在编写能够同时在CPU和GPU上运行的代码时尤其如此。以及得把GPU上的**PyTorch变量转换为Numpy数组**，这就显得有点冗长。

## **自定义扩展**

两个框架都可以构建和绑定用C、C++、CUDA编写的自定义扩展。TensorFlow仍然需要更多的样板代码，尽管这对于支持多类型和设备可能更好。在PyTorch中，你只需为每个CPU和GPU编写一个接口和相应的实现。两个框架中编译扩展也是直接记性，并不需要在pip安装的内容之外下载任何头文件或者源代码。



## TensorBoard

![](https://www.synotech.top:5523/uploads/2023/09/01/202309011045600 "")

**TensorBoard**是TensorFlow自带的可视化工具，用来查看**机器学习训练过程中数据的变化**。通过训练脚本中的几个代码段，你可以查看**任何模型的训练曲线和验证结果**。TensorBoard作为web服务运行，特别便于对于**无头结点上存储的结果进行可视化**。

第一个是**tensorboard_logger**，第二个是**crayon**。tensorboard_logger库用起来甚至比TensorBoard的“摘要”更容易，尽管想用这个首先得安装TensorBoard。



# 安装Pytorch环境

一般建议准从官网给的指导，官网链接：

安装以前的版本链接如下

[PyTorch](https://pytorch.org/get-started/previous-versions/)

比如我这里安装的指令如下：

```text
pip install torch==1.12.0+cu102 /
torchvision==0.13.0+cu102 torchaudio==0.12.0 /
--extra-index-url /
https://download.pytorch.org/whl/cu102
```

建议根据你本地安装好的CUDA环境来选择版本，否则会出现很多奇怪的异常。

安装好之后你可以使用以下命令输出torch的版本信息：

```text
import torch
print(torch.__version__)
```




***

### ***后续章节内容预告***：

***

### 引用文献：
