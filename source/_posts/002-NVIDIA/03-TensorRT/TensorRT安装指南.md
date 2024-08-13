---
title: TensorRT安装指南
date: 2022-08-24 22:54:49
updated: 2022-08-24 22:54:49
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

## ***TensorRT安装指南***

本笔记主要记录在学习TensorRT官方文档时，一些安装方式，不同的安装方式之间的区别。

### ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，同时转载请说明出处，感谢！

---

# 版本

版本发布参考archives，本文以TensorRT8.4.1进行描述:

[Documentation Archives :: NVIDIA Deep Learning TensorRT Documentation](https://docs.nvidia.com/deeplearning/tensorrt/archives/index.html)

参考官方的文档TensorRT8.4.1

[Installation Guide :: NVIDIA Deep Learning TensorRT Documentation](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#installing-debian)

TensorRT版本对照表

[Release Notes :: NVIDIA Deep Learning TensorRT Documentation](https://docs.nvidia.com/deeplearning/tensorrt/release-notes/tensorrt-8.html#rel-8-2-4)

# 安装方式

有这些方法：Using Debian or RPM packages, a pip wheel file, a tar file, or a zip file.

## 离线预编译包安装

使用官网提供的安装包：[__https://developer.nvidia.com/nvidia-tensorrt-8x-download__](https://developer.nvidia.com/nvidia-tensorrt-8x-download)

<img alt="02-TensorRT安装指南-6ff62ded.png" src="02-TensorRT安装指南-6ff62ded.png" width="" height="" >
**安装包方式注意的点：**

1. 需要sudo权限

1. 不能任意决定TenosrRT的安装位置

1. 需要有同样通过deb/rpm包安装的CUDA及cuDNN

1. 同一个主机只能安装一个小版本的TensorRT安装

**步骤如下：**

- 下载TRT本地repo，需要和你的linux系统版本及CPU架构匹配的版本

- 手动从Debian仓库安装

```shell
os="ubuntuxx04"
tag="cudax.x-trt8.x.x.x-ga-yyyymmdd"
#比如：nv-tensorrt-repo-ubuntu1804-cuda11.6-trt8.4.1.5-ga-20220604_1-1_amd64.deb

sudo dpkg -i nv-tensorrt-repo-${os}-${tag}_1-1_amd64.deb
sudo apt-key add /var/nv-tensorrt-repo-${os}-${tag}/*.pub

sudo apt-get update
sudo apt-get install tensorrt
#使用python3安装
python3 -m pip install numpy
sudo apt-get install python3-libnvinfer-dev
#Tensorflow需要安装
python3 -m pip install protobuf
sudo apt-get install uff-converter-tf
#使用ONNX
python3 -m pip install numpy onnx
sudo apt-get install onnx-graphsurgeon
```

- 验证TensorRT是否安装好了

```shell
dpkg -l | grep TensorRT
#应该会输出以下内容：
ii  graphsurgeon-tf	8.4.1-1+cuda11.6	amd64	GraphSurgeon for TensorRT package
ii  libnvinfer-bin		8.4.1-1+cuda11.6	amd64	TensorRT binaries
ii  libnvinfer-dev		8.4.1-1+cuda11.6	amd64	TensorRT development libraries and headers
ii  libnvinfer-plugin-dev	8.4.1-1+cuda11.6	amd64	TensorRT plugin libraries
ii  libnvinfer-plugin8	8.4.1-1+cuda11.6	amd64	TensorRT plugin libraries
ii  libnvinfer-samples	8.4.1-1+cuda11.6	all	TensorRT samples
ii  libnvinfer8		8.4.1-1+cuda11.6	amd64	TensorRT runtime libraries
ii  libnvonnxparsers-dev		8.4.1-1+cuda11.6	amd64	TensorRT ONNX libraries
ii  libnvonnxparsers8	8.4.1-1+cuda11.6	amd64	TensorRT ONNX libraries
ii  libnvparsers-dev	8.4.1-1+cuda11.6	amd64	TensorRT parsers libraries
ii  libnvparsers8	8.4.1-1+cuda11.6	amd64	TensorRT parsers libraries
ii  python3-libnvinfer	8.4.1-1+cuda11.6	amd64	Python 3 bindings for TensorRT
ii  python3-libnvinfer-dev	8.4.1-1+cuda11.6	amd64	Python 3 development package for TensorRT
ii  tensorrt		8.4.1.x-1+cuda11.6 	amd64	Meta package of TensorRT
ii  uff-converter-tf	8.4.1-1+cuda11.6	amd64	UFF converter for TensorRT package
ii  onnx-graphsurgeon   8.4.1-1+cuda11.6  amd64 ONNX GraphSurgeon for TensorRT package

```



## 在线网络编译包安装

- **安装CUDA network repository**，如图所示，进入[__https://developer.nvidia.com/cuda-downloads__](https://developer.nvidia.com/cuda-downloads)，这种方式适用于已经很熟TensorRT，只想快速把应用的依赖配置起来，比如，在使用容器的时候，新手适合上面的方式。

<img alt="02-TensorRT安装指南-a2f8136f.png" src="/02-TensorRT安装指南-a2f8136f.png" width="" height="" >

比如：我这里选择了，Ubuntu 18.04 x86_64 deb(network)

```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
#如果你已经有安装号的CUDA toolkit,则可以省略下面的两步
sudo apt-get update
sudo apt-get -y install cuda
```

执行上面提供的命令，即可安装CUDA在线版本，如果你本地已经安装好了CUDA，则可以省略apt install的指令，因为当下面安装TensorRT时，apt会自动下载所需要的CUDA和cuDNN依赖

- 安装TensorRT包，根据自己需要选择下面的指令

```shell
# For only running TensorRT C++ applications
sudo apt-get install tensorrt-libs
# For also building TensorRT C++ applications:
sudo apt-get install tensorrt-dev
# For running TensorRT Python applications:
python3 -m pip install numpy
sudo apt-get install python3-libnvinfer

```

- 当第一步骤中，执行CUDA network repository的安装，Ubuntu会自动安装TensorRT对应最新版本的CUDA，下面的指令可以安装老版本CUDA对应着你现在本机已经安装的TensorRT版本。

```shell
version="8.x.x.x-1+cudax.x"
sudo apt-get install tensorrt-dev=${version}

sudo apt-mark hold tensorrt-dev
如果需要升级到最新的TensorRT或者CUDA，执行下面的
sudo apt-mark unhold tensorrt-dev

```



## APP Server安装（适用于生产部署）

```shell
使用apt-get安装Debian包即可
the libnvinfer8 package (C++) plus any additional library packages
the python3-libnvinfer package (Python 3.x)

```



## 交叉编译安装

参考链接：  

[Sample Support Guide :: NVIDIA Deep Learning TensorRT Documentation](https://docs.nvidia.com/deeplearning/tensorrt/sample-support-guide/index.html#cross-compiling)

暂时不更新这一部分。



## Pip安装

注意：**While the TensorRT packages also contain pip wheel files, those wheel files require the rest of the .deb or .rpm packages to be installed and will not work alone**（意思就是，TensorRT包中也包含了pip wheels, 但是这些wheels即使你使用了Pip安装，还是需要安装tensorRT的deb/rpm包，pip包无法单独运行）。我们需要的是，**standalone pip-installable TensorRT wheel files** ，这些pip包是可以不需要先安装的TensorRT deb/rpm包的。

**The pip-installable nvidia-tensorrt Python wheel files only support Python versions 3.6 to 3.10 and CUDA 11.x at this time and will not work with other Python or CUDA versions.**

同时，这些包只能支持 **python3.6 - 3.10及 CUDA11.x**的版本，其他的版本都不行，也只能运行在Linux x86_64的系统上（CentOS7以及Ubuntu18以上的最新版本）。

- 确保nvidia-pyindex包已经安装，这个使用用来从NGC PyPI repo中获取其他附加Python模块的。(需要确保，你的pip 和setuptools不能太旧，过时，否则可能安装失败）

```shell
python3 -m pip install --upgrade setuptools pip
pip install nvidia-pyindex
#可以添加下面的依赖到你项目的requirements.txt
--extra-index-url https://pypi.ngc.nvidia.com
```

- 安装TensorRT pip包

```shell
python3 -m pip install --upgrade nvidia-tensorrt
#这个安装命令，将会同时安装依赖的CUDA和cuDNN pip包，因为这些都是TensorRT的依赖
如果安装的时候报错有类似下面的信息，那就是python版本不对/nvidia-pyindex没有安装好
##################################################################
The package you are trying to install is only a placeholder project on PyPI.org repository.
This package is hosted on NVIDIA Python Package Index.

This package can be installed as:
$ pip install nvidia-pyindex
$ pip install nvidia-tensorrt
##################################################################
```

- 验证是否安装好

```shell
python3
>>> import tensorrt
>>> print(tensorrt.__version__)
>>> assert tensorrt.Builder(tensorrt.Logger())
如果最后这个python指令报错，类似下面的输出信息，那么可能是没有安装好显卡驱动
[TensorRT] ERROR: CUDA initialization failure with error 100. Please check your CUDA installation: ...

```



## Tar包安装

1. 安装好你本机需要的CUDA（[__10.2__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.0 update 1__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.1 update 1__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.2 update 2__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.3 update 1__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.4 update 4__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.5 update 2__](https://developer.nvidia.com/cuda-toolkit-archive), [__11.6 update 2__](https://developer.nvidia.com/cuda-toolkit-archive) or [__11.7__](https://developer.nvidia.com/cuda-toolkit-archive)）

1. 安装好[__cuDNN 8.4.1__](https://docs.nvidia.com/deeplearning/cudnn/release-notes/rel_8.html#rel-841)

1. 安装好python3(可选）

1. 下载TensorRT Tar包[Download](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#downloading "Ensure you are a member of the NVIDIA Developer Program. If not, follow the prompts to gain access.")

1. 解压文件

```shell
version="8.x.x.x"
arch=$(uname -m)
cuda="cuda-x.x"
cudnn="cudnn8.x"
tar -xzvf TensorRT-${version}.Linux.${arch}-gnu.${cuda}.${cudnn}.tar.gz
#
8.x.x.x is your TensorRT version
cuda-x.x is CUDA version 10.2 or 11.6
cudnn8.x is cuDNN version 8.4

```

1. 添加TensorRT lib文件夹的绝对路径到环境变量中

```shell
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:<TensorRT-${version}/lib>
```

1. 安装python TensorRT wheel包

```shell
cd TensorRT-${version}/python
python3 -m pip install tensorrt-*-cp3x-none-linux_x86_64.whl
```

1. 安装 Python UFF wheel 包

```shell
cd TensorRT-${version}/uff

python3 -m pip install uff-0.6.9-py2.py3-none-any.whl
```

1. 安装Python onnx-graphsurgeon /graphsurgeon  wheel 包

```shell
cd TensorRT-${version}/onnx_graphsurgeon
python3 -m pip install onnx_graphsurgeon-0.3.12-py2.py3-none-any.whl
cd TensorRT-${version}/graphsurgeon
python3 -m pip install graphsurgeon-0.4.6-py2.py3-none-any.whl
```



## ZIP包安装（适用于windows)

参考官方[__https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#installing-zip__](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html#installing-zip)



# 卸载方式

1. 卸载libnvinfer8，通过deb包安装的
1. 卸载uff-converter-tf, graphsurgeon-tf, and onnx-graphsurgeon

```shell
sudo apt-get purge graphsurgeon-tf onnx-graphsurgeon
sudo apt-get purge uff-converter-tf #不卸载graphsurgeon-tf也可以
sudo apt-get autoremove
```

3. 卸载Python TensorRT

```shell
sudo pip3 uninstall tensorrt
```

4. 卸载Python UFF

```shell
sudo pip3 uninstall uff
```

5. 卸载Python GraphSurgeon

```shell
sudo pip3 uninstall graphsurgeon
```

6. 卸载Python ONNX GraphSurgeon

```shell
sudo pip3 uninstall onnx-graphsurgeon
```



# PyCUDA

确保安装了numpy

```shell
python3 -m pip install numpy
python3 -m pip install 'pycuda<2021.1'
```
