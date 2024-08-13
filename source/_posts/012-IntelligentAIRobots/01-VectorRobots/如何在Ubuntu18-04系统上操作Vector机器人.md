---
title: 如何在Ubuntu18.04系统上操作Vector机器人
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-06 11:15:32
updated: 2023-06-06 11:15:32
tags: Vector
categories: 
- 智能AI机器人
- VectorRobots
keywords: Vector
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***如何在Ubuntu18.04系统上操 作Vector机器人***

本篇主要介绍如何在Ubuntu18系统上安装Vector SDK并在局域网内成功连接上机器人，读取传感器信息以及下发指令操作机器人运动。

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，同时转载请说明出处，感谢！

---

# 安装SDK 

```text
#如果已经安装了Python3，则跳过这里
sudo apt-get update
sudo apt-get install python3
sudo apt install python3-pip

#
sudo apt-get install python3-pil.imagetk
python3 -m pip install --user anki_vector

#如果已经安装过SDK，则可以直接升级
python3 -m pip install --user --upgrade anki_vector

#这一步完成Vector的认证
python3 -m anki_vector.configure
```

# 配置Vector信息

窗口提示配置信息和隐私策略等

![image](https://www.synotech.top:5523/uploads/2023/06/06/202306061548371.jpg)

然后输入你的robot name, 比如：Vector-A1B2

![1681308582(1)](https://www.synotech.top:5523/uploads/2023/06/06/202306061549087.jpg)

输入你的机器人在局域网内wifi分配的IP地址，比如：192.168.42.42

![1681288567(1)](https://www.synotech.top:5523/uploads/2023/06/06/202306061549237.jpg)

输入你的机器人的序列号，比如：00e20100

![1681288477(1)](https://www.synotech.top:5523/uploads/2023/06/06/202306061549830.jpg)

根据官网信息，配置好了自己的Vector账号之后，可以看到Linux终端输出了SUCCESS！的信息，则配置成功。

![1681288650(1)](https://www.synotech.top:5523/uploads/2023/06/06/202306061550564.jpg)

细心观察，可以看到这里连接到的机器人主机是通过443端口的。并且对你的认证秘钥以及sdk的配置都保存在你本地的`.anki_vector/`的路径下了。

如果你成功运行到这里，看到输出，那么你已经完成了机器人和你的Ubuntu主机在局域网内连通的配置。

***

### 引用文献：

[Installation - Linux — Vector SDK 0.6.0 documentation](https://developer.anki.com/vector/docs/install-linux.html)
