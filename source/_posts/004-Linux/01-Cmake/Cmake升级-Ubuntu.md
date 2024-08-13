---
title: CMake升级-Ubuntu
date: 2022-08-27 23:47:30
updated: 2022-08-27 23:47:30
tags: "CMake"
categories: 
- Linux
- Cmake
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

# ***CMake升级方法***



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

Cmake升级是需要十分注意的，看了很多网上的博客，都是方式各异，也有不是很合理的方案。我们还是遵循自己作为一个开发者，全面了解Cmake具体的内容，再了解升级会有什么影响，再了解升级的不同种方式，最后让读者自己选择Cmake的升级策略。



### Cmake介绍

Cmake是一个什么工具？Cmake开发开始于1999年，是为了解决需要一个跨平台构建环境的 [Insight Segmentation and Registration Toolkit](https://en.wikipedia.org/wiki/Insight_Segmentation_and_Registration_Toolkit) （ITK)而开发的。



### Cmake升级的影响

首先, 如果你只是在后续的一个项目中需要使用单一版本的cmake，那么你可以完全重新编译一个在本地，但是，如果你需要保留默认安装的Cmake版本，那么需要一些特殊的操作来管理多个cmake版本在你的主机上。在我自己的开发中，如果默认就把ROS安装好了或者其他的软件在老版本的Cmake下编译好的，那么当你安装了新的cmake, 并且覆盖了老版本的cmake链接，那么将很难找回。在之前的cmake下编译好的ROS驱动和包可以继续使用，只是当你需要安装编译新的包时候，可能会出现包之间由于不同版本cmake编译的原因，导致冲突或者依赖项不兼容的问题。总之，当你需要升级Ubuntu系统的cmake，需要使用update-alternatives进行管理。



### Cmake的合理升级方法

我这里以Ubuntu18为例，默认安装的是cmake-3.10，链接在/usr/bin/cmake下，使用命令：```$ cmake --version``` 可以查看版本。默认的cmake是通过apt-get install安装的，而我们需要使用源码编译的方式来升级会更方便定义它安装的位置在哪。



1. 下载cmake源码

   ``` shell
   wget -O cmake-3.22.1.tar.gz https://cmake.org/files/v3.22/cmake-3.22.1.tar.gz
   ```

   根据自己需要的版本进行下载。

   

   ![image-20220830004817114](image-20220830004817114.png)

   



目前最新的版本是3.24，cmake是向后兼容的，但是编译好的包就无法进行修改。关于编译工具cmake的版本，个人觉得如果你项目是用来部署生产的，不需要考虑最新的安装包，选择发行stable的老一些的版本，可以避免后期加依赖的时候出现其他问题。

2. 解压并进入到cmake文件内

   ```shell
   $ tar -xvzf cmake-3.22.1.tar.gz && cd cmake-3.22.1
   ```

3. 编译源码

   ```shell
   cmake -DCMAKE_INSTALL_PREFIX=/usr .
   make
   make install ##可能需要sudo权限
   ##编译好的cmake将会把bin下的cmake可执行文件链接到 /usr/local/bin/cmake 
   ```

4. 二进制包安装

   ```shell
   # 下载二进制包
   wget https://github.com/Kitware/CMake/releases/download/v3.18.2/cmake-3.18.2-Linux-x86_64.tar.gz
   tar zxvf cmake-3.18.2-Linux-x86_64.tar.gz
   cd cmake-3.18.2
   # 将文件夹内容拷贝到默认环境变量路径下，例如/usr
   ```

5. 快捷脚本安装

   ```shell
   $ wget -q -O cmake-linux.sh https://github.com/Kitware/CMake/releases/download/v3.17.0/cmake-3.17.0-Linux-x86_64.sh
   $ sh cmake-linux.sh -- --skip-license --prefix=/usr
   $ rm cmake-linux.sh
   ```

   

6. 查看安装好的版本号

   ```$ cmake --version```
   正常的输出是对应你刚才下载的cmake版本号，因为你是用户安装，会默认放在'/usr/local/bin'路径下。

7. 使用update-alternatives添加cmake的版本
   ``` shell
   
   *# 第一个参数: --install 表示向update-alternatives注册服务名。* 
   
   *# 第二个参数: 注册最终地址，成功后将会把命令在这个固定的目的地址做真实命令的软链，以后管理就是管理这个软链；*
   
    *# 第三个参数: 服务名，以后管理时以它为关联依据。*
   
    *# 第四个参数: 被管理的命令绝对路径。*
   
    *# 第五个参数: 优先级，数字越大优先级越高。*
    
   ##添加刚安装的cmake3.22.1，cmake 后面的路径是你刚才解压后文件的路径
   sudo update-alternatives --install /usr/local/bin/cmake  cmake /home/t/Documents/cmake-3.22.1/bin/cmake  10  
   
   ##添加默认的cmake-3.10.2，默认的cmake链接在/usr/bin/cmake 
   sudo update-alternatives --install /usr/local/bin/cmake  cmake  /usr/bin/cmake  100 
   
   ##如果你重复上面的步骤，你可以再安装一个3.18.0
   tar -xvzf cmake-3.18.0.tar.gz && cd cmake-3.18.0
   #这个时候，不需要安装在/usr路径下，否则会覆盖之前安装的版本
   ./bootstrap
   make
   #make编译之后，就已经把源码编译好了，会生成一个/bin目录，cmake的执行文件就在/bin下
   
   ##添加默认的cmake-3.18.0，默认的cmake链接在解压后的源码根目录的/bin下，比如我这里是 /home/t/Documents/cmake-3.18.0/bin/cmake
   sudo update-alternatives --install /usr/local/bin/cmake  cmake /home/t/Documents/cmake-3.18.0/bin/cmake  1  
   
   这样我们就添加了三个cmake版本，两个是你刚才在用户下安装的，一个是默认的cmake deb包安装的。
   
   ```

   如图所示：安装了三个版本的cmake，优先级可以自己定义，我这里定义默认的automode选择优先级最高的cmake-3.10.2，也就是/usr/bin/cmake链接的默认的cmake。

   ![image-20220830010853110](image-20220830010853110.png)

   可以自己使用--config 进行配置，选择你需要使用的cmake版本，当你项目需要使用高版本，你可以切换到高版本进行编译，需要低版本，就再切换到低版本进行编译，cmake之间的链接不会冲突干扰。

   ![image-20220830010646685](image-20220830010646685.png)



### 可能的误操作

之前，不是很熟悉cmake管理的时候，会自己编译安装，但是发现安装之后，覆盖了原有deb包安装的cmake-3.10.2的链接，而且我还找不到，因为是deb安装的方式，没有源码就无法复制一个cmake执行文件，重新给个链接，导致需要卸载再重新安装，可能有其他方法，但是目前我还不知道怎么操作。因为需要卸载，

卸载方式指令：```$ sudo apt remove cmake```, 在不清楚 ```$ sudo apt autoremove cmake```会产生什么效果之前，小心使用，一般都会在命令行提醒你，apt将会卸载哪些包。在我这里的情况是，因为ROS是在cmake-3.10下安装的，所以cmake卸载的话，会同步把ROS的安装包和驱动都一并的卸载，这也是很多小白最初不会操作的时候，经常犯错的。

如果你确实没有办法恢复老版本的cmake，就像我这里一样，使用```$ sudo apt remove cmake```卸载cmake3.10，然后再使用 ```$ sudo apt autoremove cmake```看提示需要卸载什么ROS包，如果无关紧要，就执行autoremove的指令。最后，重新安装cmake，```$ sudo apt  install cmake```, 重新安装那些刚才你卸载的包，比如ROS。




***

### ***后续章节内容预告***：

***

### 参考引用文献：

https://turbock79.cn/?p=2582 
