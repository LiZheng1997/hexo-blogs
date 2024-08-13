---
title: Autoware.Universe环境搭建
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-08 16:17:05
updated: 2023-06-08 16:17:05
tags: [Autoware.Universe, Docker]
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

# ***Autoware.Universe环境搭建***



## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请说明出处引用，感谢！

---

# 1. Docker镜像安装

两种镜像：**预编译（prebuilt）**和**开发版(devlopment)**

**预编译**中的版本，可以适合小白迅速上手测试，无需关心代码的编译，驱动依赖等问题，镜像内部已经有编译好的代码及所有的依赖项环境。

**开发版**，适合开发者部署自己的代码，进行开发工作，需要关心编译问题，以及其他的依赖项可能缺失的问题，镜像内部没有编译好的代码，好处就是，镜像相对较小，但是代码需要自己编译和处理可能有的bugs。



## 拉取源码

```text
git clone https://github.com/autowarefoundation/autoware.git
cd autoware
```

## 创建工作空间

需要创建一个autoware_map的文件夹来保存后面将会使用到的地图。这样我们就有了一个工作空间，其中包含了autoware的源码和autoware_map的地图信息。

```text
mkdir ~/autoware_map
```

## 拉取镜像

```text
docker pull ghcr.io/autowarefoundation/autoware-universe:latest-cuda
```

根据自己的情况选择合适的镜像，这里默认的latest-cuda镜像是基于ubuntu22上构建的支持amd64硬件的镜像。

## 启动容器

```text
rocker --nvidia --x11 --user --volume $HOME/autoware --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
```

rocker的具体使用方法可以参考[__这里__](https://github.com/autowarefoundation/autoware/tree/main/docker/README.md)

如果是使用arm64的架构平台需要使用不同的启动方式：

```text
rocker -e LIBGL_ALWAYS_SOFTWARE=1 --x11 --user --volume $HOME/autoware -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
```

## docker的启动指令

```text
docker run --rm -it  --gpus all 
-v /home/userroot/autoware:/home/userroot/autoware
-v /home/userroot/autoware_map:/home/userroot/autoware_map  
-e DISPLAY -e TERM   -e QT_X11_NO_MITSHM=1   
-e XAUTHORITY=/tmp/.dockercnqq0upg.xauth 
-v /tmp/.dockercnqq0upg.xauth:/tmp/.dockercnqq0upg.xauth   
-v /tmp/.X11-unix:/tmp/.X11-unix   
-v /etc/localtime:/etc/localtime:ro  b3ea2527886b
```

启动之后，我们就创建了一个容器并进入到容器中的终端进行交互。

## 拉取源码并安装依赖

```text
cd autoware
mkdir src
vcs import src < autoware.repos
sudo apt update
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

## 编译工作空间

```text
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-w"
```

我这里编译报错的信息

```text
11 packages had stderr output: behavior_path_planner elevation_map_loader lidar_apollo_segmentation_tvm lidar_apollo_segmentation_tvm_nodes lidar_centerpoint_tvm map_tf_generator obstacle_velocity_limiter pacmod_interface system_monitor tensorrt_yolox velodyne_pointcloud
```

编译完成就可以启动autoware

## 启动autoware

```text
source install/setup.bash
ros2 launch autoware_launch e2e_simulator.launch.xml vehicle_model:=sample_vehicle sensor_model:=awsim_sensor_kit map_path:=<your mapfile location>
```

需要关联仿真环境才能够完成启动上面的指令，需要先启动仿真环境。

## 启动仿真

```text
ros2 launch autoware_launch e2e_simulator.launch.xml vehicle_model:=sample_vehicle sensor_model:=awsim_sensor_kit map_path:=/home/userroot/autoware_map/nishishinjuku_autoware_map
```

参考AWSIM的部分

[Ubuntu18安装AWSIM运行autoware.universe](https://www.bluenote.top/2023/06/08/008-%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6/02-Autoware.universe/Ubuntu18%E5%AE%89%E8%A3%85AWSIM%E8%BF%90%E8%A1%8CAutoware-Universe/)







# 2. 源码编译安装

挖坑代填。。









# 参考引用

Docker镜像仓库

[Build software better, together](https://github.com/autowarefoundation/autoware/pkgs/container/autoware-universe)

rocker使用介绍

[GitHub - osrf/rocker: A tool to run docker containers with overlays and convenient options for things like GUIs etc.](https://github.com/osrf/rocker)
