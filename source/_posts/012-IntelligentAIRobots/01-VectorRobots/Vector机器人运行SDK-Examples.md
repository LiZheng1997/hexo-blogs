---
title: Vector机器人运行SDK-Examples
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-07 23:39:52
updated: 2023-06-07 23:39:52
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

# ***Vector机器人运行SDK-Examples***

介绍并演示使用Vector机器人提供的一些SDK的案例代码，执行之后的效果。

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请说明出处引用，感谢！

---

# 启动3d_viewer.py脚本

![3d_view](https://www.synotech.top:5523/uploads/2023/06/08/202306080007457.jpg)

这个脚本将会创建一个图像窗口和3D OpenGL的窗口，显示了当前机器人通过一线Laser激光扫描创建的地图，同时在图像窗口，如果看到CUBE的话，会显示检测的CUBE信息，并画出矩形框。

# 17_create_wall.py

运行Demo17, 将会在机器人的正前方100mm处创建一堵墙，然后下指令让机器人穿过墙，前往机器人正前方200mm处。

代码如下：

![image](https://www.synotech.top:5523/uploads/2023/06/07/202306072349464.jpg)

3d_viewer创建可视化情况如下：

![3d_viewer](https://www.synotech.top:5523/uploads/2023/06/07/202306072350142.jpg)

机器人绕过前方虚拟的墙，然后到达距离机器人正前方200mm处。

***

### 引用文献：
