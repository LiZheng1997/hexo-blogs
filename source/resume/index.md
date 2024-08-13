---
title: resume
toc: true
toc_number: true
comments: false
date: 2023-04-27 01:08:05
password: Lz-Bluet
---

 <center>
     <h1>Bluet</h1>
     <div>
         <span>
             {% inlineImg src="index/phone-solid.svg" width="18px" %}
             17269651686
         </span>
         ·
         <span>
             {% inlineImg src="index/envelope-solid.svg" width="18px" %}
             1429053840@qq.com
         </span>
         ·
         <span>
             {% inlineImg src="index/github-brands.svg" width="18px" %}
             <a href="https://github.com/Bluet1997">Bluet</a>
         </span>
         ·
         <span>
             {% inlineImg src="index/rss-solid.svg" width="18px" %}
             <a href="https://bluenote.top">My Blog</a>
         </span>
     </div>
 </center>



 ## {% inlineImg index/info-circle-solid.svg 30px %} 个人信息 

 - 男，1996/12，汉族
 - 工作经验：3 年

## {% inlineImg src="index/graduation-cap-solid.svg" width="30px" %}教育经历

- 硕士，谢菲尔德大学，高级计算机科学(一等一学位 73/100)，                            2018.9 ~ 2020.3
- 学士，江西农业大学，软件工程+英语专业(双学位 83/100)，                              2014.9~2018.7
- 专业英语四级，四六级，雅思，GRE

## {% inlineImg  src="index/briefcase-solid.svg" width="30px"%} 工作经历

- **韵达东普科技公司，无人车/智能仓研究所，算法工程师，2020.05 - 2021.04**

  负责智能仓机械臂视觉算法，快递分拣及单件分离项目，快递无人车视觉算法。

- **贵州翰凯斯科技(Pixmoving)，自动驾驶RD部门，自动驾驶算法工程师，2021.04 - 2023.02**

  负责半封闭园区L4无人车自动驾驶视觉算法，Robobus/清扫车/定制化项目研发。

## {% inlineImg src="index/project-diagram-solid.svg" width="30px"%} 项目经历

- **PIXMoving-清扫车 垃圾检测/实例分割/路沿检测/多传感器融合**   				                          2022.03– 2022.09

  **平台**: 自研清扫车 **技术点**: 基于ZED2i相机，使用YOLO-V5和Sparseinst算法，从数据收集/清洗/标注/训练到完成ROS/TensorRT部署，整个项目的链路搭建到部署都是由我个人完成，路沿检测研发基于激光雷达和ZED2i相机落地部署。

- **PIXkit纯视觉跟踪**                                                                                                                   2021.12 – 2022.01

  **平台:** 自研自动驾驶套件+云台相机 **技术点**: 使用YOLOX和SiamFC++算法，完成对行人、车辆、无人机目标的自动跟踪系统的落地。

- **Robobus研发** 																								                       2021.07 – 2023.02

  **平台**: 自研Robobus **技术点**: **相机选型**及**布局**设计，**外参/内参**标定，**时间同步**，基于**多相机BEV**感知算法(Transformer)完成3D感知任务的研发，多传感器融合方案的**功能设计**，基础自动驾驶框架基于**Autoware**优化部署的。

- **PIXMoving-KUKA机械臂视觉** 																	                           2021.07 - 2023.02

  **平台**: KUKA-KR210 **技术点**: 机械臂**Gazebo**模型构建，**3D结构光**相机选型，实时**RTM-3D**成形件**CAD误差检测**自动化流程设计。

- **PIXkit-Autoware感知板块**                                                                                                      2021.04– 2021.07

  **平台**:自研自动驾驶平台 **技术点**: 完整Autoware技术栈部署，2D/3D感知算法部署优化，所有交付的Pixkit项目均基于此发货。

- **快递无人车视觉研发(韵达研究所-无人车)**                                                                              2020.09 – 2021.03

  **平台**: 自研快递无人车 **技术点**：**联合标定**，**障碍物 2D 检测**，**点云 3D 检测(应用)**，**相机和激光雷达融合**(障碍物检测)。**最终效果**：在 Jetson 开发板上实现了 Mask RCNN/YOLO 实时目标检测，借助 TensorRT 加速/DeepStream，使用docker完成部署。

- **机械臂 3D 视觉应用(韵达研究所-智能仓)**                                                                               2020.07 – 2020.09

  **平台**: 自研 6 轴机械臂 **技术点**：研究前沿的 **6D 姿态估计算法**，借助英伟达 **Isaac 平台**复现算法并实现场景的应用落地。**最终效果**：对比测试了 **PVNet/PoseCNN** 模型，在 **Jetosn Xavier NX** 上实现了 **PoseCNN** 模型的部署，完成码垛任务。[Demo链接](https://v.youku.com/v_show/id_XNDg1NjYzNTA4MA==.html?spm=a2hcb.profile.app.5~5!2~5~5!3~5!2~5~5~A)

- **分拣单件分离 2D 机器视觉研发(韵达研究所-智能仓)**                                                             2020.06 – 2020.07

  **应用场景**: 物流分拨中心包裹分拣 **技术点**：应用**OpenCV**库进行传送带上**单件多件包裹**的识别。**最终效果**：配合六面扫，完成摆臂的单件分离的功能实现，传送带速度 **1.5m/s** 左右。

- **仿生机器人视觉导航控制系统**                                                                                                2019.06 – 2019.09

  **机器人平台**：谢菲尔德机器人实验室，仿生机器人**MiRo**（社交陪伴型机器人）。**技术点**：使用级联分类器（**LBP和HOG**）配合**SVM**支持向量机和神经网络模型（**MobileNet-SSD**等深度学习架构）实现目标检测和识别。使用**OpenCV**库函数，选择**TLD tracker**进行目标跟踪。使用**Braitenberg vehicle**仿生理论进行移动控制。**最终效果**：机器人能够自动的识别对应的分类目标（设计了三种类型目标），进行自主的**拦截行为**，目标是视野内**随机的移动目标**。同时，简化版本是进行**目标的跟随**。[Demo1链接](https://v.youku.com/v_show/id_XNDUyNDI3MTcwNA==.html?spm=a2hzp.8253869.0.0) [Demo2链接](https://v.youku.com/v_show/id_XNDUyNDI3MDc3Mg==.html?spm=a2hzp.8253869.0.0)

## {% inlineImg src="index/activity.svg" width="30px"%} 实习经历

- 谢菲尔德Jaguar Robot实习                                                                                                      2019.12-2020.03

  **内容**：部署一个视觉导航项目在Jaguar robot上，在**Dr Li Sun**的带领下完成了一个**自动化导航**的	系统部署。[Demo链接](https://v.youku.com/v_show/id_XNDUyMDIyMzU5Ng==.html?spm=a2h3j.8428770.3416059.1)

## {% inlineImg src="index/academy.svg" width="30px"%} 学术经历

- *Application of Computer Vision for A Biomimetic Robot* （**Dissertation Project**)
- *A Self-supervised, Self-adaptive Model for the Next Generation Vision-based Robot Navigation* (**Research Proposal**)

## {% inlineImg src="index/social-activity.svg" width="30px"%} 社会活动

- **谢菲尔德AMRC工厂“UK-RAS Manufacturing Robotics Challenge”挑战赛**

  负责完成控制**LBR KUKA iiwa**机器臂，进行拾取和放置的任务。

## {% inlineImg src="index/personality.svg" width="30px"%} 兴趣爱好

​	业余健体爱好者，爱好电子科技产品、撸各类机器人，动手实践能力强。
