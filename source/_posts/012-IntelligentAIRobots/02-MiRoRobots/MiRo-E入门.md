---
title: MiRo-E入门
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-10-11 14:29:19
updated: 2023-10-11 14:29:19
tags: MiRo
categories:
- 智能AI机器人
- MiRoRobots
keywords: MiRo
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***MiRo-E入门***

# 背景

MiRo-E机器人是谢菲尔德大学机器人实验室的一块陪伴型仿生机器人，它具有仿生的行为和语音，能够根据和人类的交互产生不同的反馈，具有适应性的行为表现。MiRo-E目前已经适配到了Ubuntu20的版本，对应的是ROS-Noetic的版本，熟悉ROS的同学可以很快上手。机器人本体具有多种类型的传感器：双目相机、声呐传感器、光传感器、麦克风声音传感器、触感传感器、跌落检测传感器以及轮式编码器集成在步进电机内。需要更多传感器信息的同学可以参考：

[MIRO-E: Sensors](http://labs.consequentialrobotics.com/miro-e/docs/index.php?page=Technical_Sensors)

实验室拍摄的机器人：

![IMG_20190213_151708](https://www.synotech.top:5523/uploads/2023/10/11/202310111650895.jpg)



安装步骤清单：

1. 安装ROS1/2

1. 安装MDK环境

1. 测试MDK安装情况



# 安装ROS1

ROS的安装步骤，根据自己的主机系统版本进行选择，官网有很详细的安装步骤，也可以参考我之前的博客进行安装，链接：



也可以参考MiRo官方的指导：

[MIRO-E: Install ROS](http://labs.consequentialrobotics.com/miro-e/docs/index.php?page=Developer_Install_Steps_Install_ROS)

这里我演示MiRo-E官网安装ubuntu20上的ros-noetic版本。官网也有一行命令脚本快速安装：

```text
wget -c https://raw.githubusercontent.com/qboticslabs/ros_install_noetic/master/ros_install_noetic.sh && chmod +x ./ros_install_noetic.sh && ./ros_install_noetic.sh
```

如果安装成功，source下环境就可以启动了roscore。



# 安装MDK

安装MDK的依赖如：

```text
$ sudo apt install build-essential python3-pip
$ pip install apriltag
$ sudo apt install python3-matplotlib python3-tk ffmpeg

```

下载MDK，根据自己的系统架构来选择版本，下载完成解压到自己定义的路径下，一般放在~/路径就可以，解压后执行下面的操作：

```text
$ cd ~/mdk-190211/bin/deb64
$ ./install_mdk.sh
```

下载包的时候需要你提交下自己的信息，才会开始下载，如图填好信息提交：

![1696731223](https://www.synotech.top:5523/uploads/2023/10/11/202310111720406.jpg)

笔者现在看到的版本是mdk_230105的版本了，之前是20190211。



# 测试MDK

测试下安装MDK的情况，如果有类似如下输出就是安装好了。其中MDK release是对应你自己安装的版本号，MIRO的edition也是。

```text
Sourcing mdk/setup.bash...

MIRO edition: 2
MDK path: ~/mdk
MDK release: R190518
User setup: ~/.miro2/config/user_setup.bash

Local network address: 192.168.1.100 (set from miro_get_dynamic_address())
Robot network address:  (not set)
ROS master address: http://localhost:11311 (not set, assumed running locally)

Type "miro_info" to see your environment
________________________________________________________________
```



# 配置MDK

这个配置一般前面正常安装输出了信息，在上面的测试MDK安装情况的时候，那就不需要额外配置。这里配置的内容可以参考官网，我举例说明：

官网也做了提示，在大多数情况下，都不需要这步骤的额外配置，

In most cases, no configuration changes will be required at all—read [Install MDK](http://labs.consequentialrobotics.com/miro-e/docs/index.php?page=Developer_Install_Steps_Install_MDK) carefully, and determine if you need to make changes before continuing with the instructions on this page.

## 配置的文件

可以配置的文件路径： ~/.miro2/config/user_setup.bash. 这里面提到的配置参数都可以修改，但是你修改之前必须知道它的含义。

安装的时候会创建一个~/mdk的超链接。这个超链接会链接到你下载的包放置的文件路径。

安装MDK的过程中，会把source ~/mdk/setup.bash 配置到~/.bashrc中。

这个source的环境配置大多数人都会删除掉，你可以删除之后，自己在每次使用MDK之前把环境重新source一遍。



# 网络的配置

如果你是在本地电脑上进行仿真和MDK一起配置使用，那么网络的配置就不需要额外的设置，MDK中已经在配置文件里进行自动网络Local network address的配置，如果是使用机器人本体进行测试，就需要把机器人的网络地址配置到~/.miro2/config/user_setup.bash中。



关于我的硕士答辩项目，可以参考我的MiRo答辩项目-2019的博客：



Demo视频如下：

<iframe width="560" height="315" src="https://www.youtube.com/embed/O_p8CYiN4_s?si=kBjI4p2nZnI149sP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SpyKzYiWuG8?si=E-nNVPwEV19MeNzw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>





***

### 引用文献：

[MIRO-E: Developer](http://labs.consequentialrobotics.com/miro-e/docs/index.php?page=Developer)
