---
title: Ubuntu18安装AWSIM运行Autoware.Universe
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-08 00:08:53
updated: 2023-06-08 00:08:53
tags: [Autoware.Universe, Ubuntu18, AWSIM]
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

# ***Ubuntu18安装AWSIM运行Autoware.Universe***

主要介绍如何在Ubuntu18主机系统上快速配置好仿真环境以及使用autoware.universe在docker容器中进行车辆自动驾驶。

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请说明出处引用，感谢！

---

# 效果图

![Image_top](https://www.synotech.top:5523/uploads/2023/06/08/202306080127094.jpg)



# 仿真配置情况

![image-20230608012930509](https://www.synotech.top:5523/uploads/2023/06/08/202306080129751.jpg)

# 环境要求

## 主机系统

如果打算在主机系统本地运行仿真，则需要参考官方推荐，使用20或者22的ubuntu系统。

选择Ubuntu20需要选择**1.0.2**的版本

[AWSIM/index.md at v1.0.2 · tier4/AWSIM](https://github.com/tier4/AWSIM/blob/v1.0.2/docs/GettingStarted/QuickStartDemo/index.md)

ubuntu20使用的是ROS2 Galactic版本。

选择Ubuntu22可以选择**1.1.0**版本

## 硬件要求

![image-20230608013036319](https://www.synotech.top:5523/uploads/2023/06/08/202306080130564.jpg)

经过测试内存如果在16GB，显存是8GB可能已经是最小能运行的配置了，因为我实际使用这个配置中，会出现仿真中画面卡顿，不流畅的问题。

如果需要在本地把bashrc的配置更改一下，如下：

```text
export ROS_LOCALHOST_ONLY=1
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

if [ ! -e /tmp/cycloneDDS_configured ]; then
    sudo sysctl -w net.core.rmem_max=2147483647
    sudo ip link set lo multicast on
    touch /tmp/cycloneDDS_configured
fi
```

## 安装显卡驱动和Vulkan Graphics Library

```text
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo ubuntu-drivers autoinstall
##如果你没有安装过显卡驱动，第一次安装驱动都需要重启下
sudo reboot
##使用nvidia-smi查看显卡驱动等信息
安装Vulkan
sudo apt install libvulkan1
```

## 下载仿真器的二进制文件

以下链接下载：

[__https://github.com/tier4/AWSIM/releases/download/v1.1.0/AWSIM_v1.1.0.zip__](https://github.com/tier4/AWSIM/releases/download/v1.1.0/AWSIM_v1.1.0.zip)

解压文件到$HOME路径下，

解压出文件，执行AWSIM_demo.x86_64，就可以启动仿真环境，***这里我们先不启动，因为我们后面要在容器内启动***。

## 下载地图

使用以下链接下载地图：

[__https://github.com/tier4/AWSIM/releases/download/v1.1.0/nishishinjuku_autoware_map.zip__](https://github.com/tier4/AWSIM/releases/download/v1.1.0/nishishinjuku_autoware_map.zip)

下载完后，创建一个文件夹，解压地图到里面

```text
mkdir ~/autoware_map
解压的地图文件放在这个文件夹下
```

# 拉取源码

```text
git clone https://github.com/autowarefoundation/autoware.git
cd autoware
需要切换到awsim-stable分支下
git checkout awsim-stable
```

# 安装主机依赖项

1. CUDA

1. Docker Engine

1. NVIDIA Container Toolkit

1. Rocker

你可以选择自己手动以此安装好上面的依赖项，也可以使用脚本的方式安装，在容器内已经都有了相关的环境，所以不需要安装了。在主机系统上安装的话可以以下使用脚本安装以上依赖

```text
./setup-dev-env.sh
```

# 拉取镜像并启动

```text
docker pull ghcr.io/autowarefoundation/autoware-universe:latest-cuda
rocker --nvidia --x11 --user --volume $HOME/AWSIM_v1.1.0 --volume $HOME/autoware --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:latest-cuda
```

创建src文件夹并克隆代码进来

```text
mkdir src
vcs import src < autoware.repos
```

# 安装ROS依赖包

```text
source /opt/ros/humble/setup.bash
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

# 编译工作空间

```text
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-w"
```

# 启动Autoware

```text
source install/setup.bash
ros2 launch autoware_launch e2e_simulator.launch.xml vehicle_model:=sample_vehicle sensor_model:=awsim_sensor_kit map_path:=/home/xx/autoware_map/nishishinjuku_autoware_map
```

**BUGS**

启动的时候，地图路径需要按照绝对路径填写，不能使用~/map_path这样的相对路径。

上面的步骤都顺利的话，你将会看到rviz在你主机系统上出现以下窗口内容：

![Image_2](https://www.synotech.top:5523/uploads/2023/06/08/202306080127254.jpg)

如果你可以看到在地图上有车辆和电云等初始位置的信息，那么你成功完成了autoware和仿真环境的关联。

接下来你给定一个目标点，然后给出一个启动的信号如下指令：

```text
cd autoware
source install/setup.bash
ros2 topic pub /autoware/engage autoware_auto_vehicle_msgs/msg/Engage '{engage: True}' -1
```

车辆将会自动运行到你设定的目标点位置。



其中遇到的一些上述没有说的问题，可以参考以下链接去找解决方案，这里面包含了一些常见的问题。

[__https://tier4.github.io/AWSIM/DeveloperGuide/TroubleShooting/__](https://tier4.github.io/AWSIM/DeveloperGuide/TroubleShooting/)



正确运行起来之后，我在笔记本GPU是2070-MAXQ和Ubuntu18.04上使用官方提供的镜像和awsim-stable代码进行演示，效果如下：

![image-20230608012831843](https://www.synotech.top:5523/uploads/2023/06/08/202306080128339.jpg)



# 参考引用

**参考官网链接**

[Quick Start Demo - AWSIM document](https://tier4.github.io/AWSIM/GettingStarted/QuickStartDemo/)

