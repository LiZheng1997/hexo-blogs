---
title: 3步实现在Jetson Orin上联调Autoware.Universe和Carla-0.9.15
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2024-03-15 01:49:34
updated: 2024-03-15 01:49:34
tags: [Autoware.Universe, Ubuntu22, Carla]
categories:
- 自动驾驶
- Autoware.Universe
keywords:
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***如何3步实现在Jetson Orin上联调Autoware.Universe和Carla-0.9.15***



## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请说明出处引用，感谢！

---

## 准备工作

### 软件

1. Carla 0.9.15 (the newest version)
2. Autoware.universe (V1.0 Branch, will be changed to other version)
3. Autoware-Carla-Bridge (Main Branch)
4. ROS2 (Humble version)
5. Ubuntu 22.04 (LTS Released version)
6. Docker

### 硬件

Jetson Orin开发者套件在机器人和自动驾驶领域都是非常好的一个开发套件

1. 一个全千兆的交换机或者路由器（局域网内主机和Jetson Orin通信需要）
2. 一个Jetson Orin开发套件 32G/64G，带外置NVME的500G的SSD硬盘
3. 一个Ubuntu22.04的主机
4. 两个或以上的超五类或者六类网线

所有的设备必须在局域网内相互连接能够通信，全千兆的规格会更好。

------

***Note:*** 为了说明方便，我使用“主机”来表示带有GPU显卡(2070或者30/40系列的显卡均可)的机器。这个主机将会用来部署Carla-0.9.15仿真模拟器和Autoware-Carla-Bridge ROS2所有包。同时，使用Jetson Orin来表示嵌入式系统，这个嵌入式开发套件将会用来部署Autoware.Universe的技术栈软件。

------

> 在你开始下面的步骤之前，我建议你使用下面的指令先拉取一些必要的docker镜像，因此在你阅读完这个教程之后，立马可以开始部署:

```bash
 $ docker pull carlasim/carla:0.9.15
 $ docker pull tumgeka/carla-autoware-bridge:latest
 $ docker pull 1429053840/autoware.universe-carla-0.9.15:humble-20240215-cuda-arm64-v0.1
```



## 3步快速部署

### 步骤一：部署carla-0.9.15仿真器

这个步骤时基于carla官方发布的docker镜像，你需要拉取这个镜像然后在你的主机上运行起来。必须检查下rpc-port端口是否被占用。

```bash
$ docker pull carlasim/carla:0.9.15
$ docker run --privileged --gpus all --net=host -e DISPLAY=$DISPLAY carlasim/carla:0.9.15 /bin/bash ./CarlaUE4.sh -carla-rpc-port=1403
```

### 步骤二：部署autoware-carla-bridge

这一步主要是基于TUM团队发布的镜像，在我的笔记本上需要设置timeout为一个更大的值，比如10000才可以，默认值是5000ms。

```bash
$ docker pull tumgeka/carla-autoware-bridge:latest
$ docker run -it -e RMW_IMPLEMENTATION=rmw_cyclonedds_cpp --network host tumgeka/carla-autoware-bridge:latest
$ ros2 launch carla_autoware_bridge carla_aw_bridge.launch.py port:=1403 town:=Town10HD timeout:=10000
```

### 步骤三：部署autowarea.unvierse技术栈到Jetson Orin上

编译autoware.universe有时候很困难，因为有很多不同的依赖项。所以，我建议使用我发布的预编译的docker镜像来的更简单。下面的命令必须在Jetson Orin上运行。主要是因为我们想使用一个Jetson Orin设备来部署autoware.universe技术栈。

***注意：***这个镜像有点大，并且只能在ARM64的平台上使用，所以Jetson系列的开发套件都能适用。

```bash
$ docker pull 1429053840/autoware.universe-carla-0.9.15:humble-20240215-cuda-arm64-v0.1
$ sudo apt-get install python3-rocker
$ rocker --user --nvidia --privileged --network host --x11 --volume $HOME/Documents  --volume $HOME/autoware -- 1429053840/autoware.universe-carla-0.9.15:humble-20240215-cuda-arm64-v0.1
$ ros2 launch autoware_launch e2e_simulator.launch.xml vehicle_model:=carla_t2_vehicle sensor_model:=carla_t2_sensor_kit map_path:=/autoware1.0_ws/Town10/
```

> ***注意***：当你使用rocker命令的时候，可能会出现如下消息：***invoking the NVIDIA Container Runtime Hook directly (e.g. specifying the docker --gpus flag) is not supported***.
>
> 你只需要更改"--gpus all" 成 "--runtime=nvidia"，如下面的样例指令，你的指令将会和我的不一样，你应该在运行 ***rocker --user --nvidia xxxx** 命令后，粘贴终端反馈的指令信息来运行镜像。

```bash
$ docker run  --rm -it --network host   --runtime=nvidia --privileged  -e DISPLAY -e TERM   -e QT_X11_NO_MITSHM=1   -e XAUTHORITY=/tmp/.dockerafc7hfmf.xauth -v /tmp/.dockerafc7hfmf.xauth:/tmp/.dockerafc7hfmf.xauth   -v /tmp/.X11-unix:/tmp/.X11-unix   -v /etc/localtime:/etc/localtime:ro  8ea8cd5cadfe
```

 

### 可选项

#### 生成NPC

如果你想添加一些NPC到你的carla仿真器中，你需要使用一个在autoware-carla-bridge docker容器中的Python脚本来启动。同时，注意要和之前一样使用相同的端口。这个**generate_traffic.py**应该步骤二启动的docker容器中的终端中运行。

```bash
$ python3 src/carla_autoware_bridge/utils/generate_traffic.py -p 1403
```

#### 部署autoware.universe技术栈在主机上

在你的主机本地终端上编译autoware的源码，如果你不想部署在jetson嵌入式平台的话。

```bash
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

如果你想在有GPU显卡的主机上编译源码，你可能需要解决一些依赖问题。





## 本地部署工作流

本地部署工作流将会后来添加，添加一个Dockerfile，发布修改之后的源码。



## TODO List

- [ ] 添加另一个shuttle-bus的车辆模型到carla仿真器中
- [ ] 简化Dockerfile给用户进行本地Autoware.universe镜像的编译
- [ ] 上传修改的autoware.universe代码，解决感知模块无法启动的一些问题



## 解决Bugs

docker运行时的错误，错误信息可能和如下的相似：

```bash
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: Auto-detected mode as 'csv'
invoking the NVIDIA Container Runtime Hook directly (e.g. specifying the docker --gpus flag) is not supported.Please use the NVIDIA Container Runtime (e.g. specify the --runtime=nvidia flag) instead.: unknown.
```

你必须使用"--runtime=nvidia "标志莱欧启动一个容器。



## 引用

TUM论文：https://arxiv.org/abs/2402.11239

