---
title: Gimbal-Camera-Tracking
date: 2022-08-24 02:25:51
updated: 2022-08-24 02:25:51
tags: Gimbal Camera Tracking
categories: 
- Projects
- 视觉跟踪
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
mathjax: true
---

## 系统部署方式介绍
**此项目还在迭代中，部署方式采用本地host环境结合Docker环境进行算法的部署。
以后将会优化镜像部署，不使用本地环境，这样部署效率更高。**

源码将要分成两部分分别部署在主从机上。**  

---
### Host主机部署

**主机上需要部署的源码是：fv_tracking, gimbal_control, serial_ros, bboxes_ex_msgs进行部署。**  
**创建一个gimbal_camera_ws工作空间, 将刚才从gitlab上拉取的源码中以上提到的四个包放在**  
**gimbal_camera_ws的src文件夹中。**

1. **编译以上的gimbal_camera_ws的源码，在当前工作空间的根目录下执行命令：**
`$ catkin_make`

2. **编译完成后，链接好相机的USB和串口的USB转换头到工控机上，使用命令：**  
`$ ls /dev/video* ` `$ ls /dev/ttyUSB0`**去确认是否已经读取到云台相机和云台串口。**  

***备注：在代码中，默认启动和绑定的设备是" /dev/video0" 和 "/dev/ttyUSB0"， 工控
机可能需要将/ttyUSB0的读写权限打开，否则可能会报错IOException***

---

### Docker从机部署
**Docker镜像目前存储在公司的Harbor仓库中，当前维护版本镜像名为**  
**pix/gimbal-camera-tracking/pytorch-siamfc-yolox-ros-noetic/amd64:V1.1**

1. **实例化一个容器，镜像构建容器的启动命令：**  
`$docker run -it --rm --gpus all
--net host --privileged --ipc=host --volume /home/t/gimbal-camera-tracking/
gimbal_camera_ws/:/workspace/gimbal_camera_ws  xxx镜像名`  

2. **启动之后会进入到容器的内部，然后会将本地的源码映射到docker环境中的/workspace**  
**文件夹下的/gimbal_camera_ws**

***备注：在镜像构建容器的启动命令时，需要加入自己将源码放置的路径，如上命令中，"/home/t/gimbal-camera-tracking/gimbal_camera_ws"就是工控机默认将源码***  
***放在/home/t/路径下，源码的拉取下来的文件夹名为"gimbal-camera-tracking"***   

---

### 系统启动方式

**完成上面的主从机的部署后，确认主机的gimbal_camera_ws和从机docker中映射的**  
**gimbal_camera_ws中的源码都已经编译好了，然后将"source /home/t/**  
**gimbal_camera_ws/devel/setup.bash"和"source /workspace/gimbal_camera_ws/devel/setup.bash"**  
**分别添加到主机和docker从机的"~/.bashrc"环境中, 重新source下环境，以初始化环境变量的配置。**  

**接下来是启动步骤：**  

1. **检查是否可以读取到相机图像和串口信息**  
    使用`$ cheese`或者`$ rosrun  fv_tracking web_cam`,命令来读取相机图像，看是否能够打开web_cam节点并读到云台相机图像。  
    使用`$ rosrun serial_ros serial_node` 和`$ rosrun serial_ros moni`，看是否能够读取到云台的姿态信息输出。  
    确认都能读取到数据之后，将cheese关闭，**serial_node和moni就不用关闭。**
    如图：  
<center class="half">
    <img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291508803.png" width="200" height="200"/><img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291510788.png" alt="20220114-cddd0cdf" width="200" height="200"/><img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291511050.png" alt="20220114-923e402c" width="200" height="200"/>
</center>





2. **启动相机节点**  
    在主机中，使用命令`$ rosrun  fv_tracking web_cam`，启动了相机视频流读取节点。

3. **启动YOLOX的ROS节点**  
    在docker中，使用命令`$ roslaunch yolox_ros yolox_ros.launch`，启动了检测器，检测实时图像中的物体。

4. **启动SiamFC++的ROS节点, 确认你需要跟踪的目标在当前相机的视野范围内**  
    在docker中，使用命令`$ roslaunch siamfc_ros siamfc_ros.launch`，跟踪器成功锁定需要被跟踪的目标。

5. **启动主机中的fv_tracking包中的track_kcf的ROS节点, 确认被跟踪的物体是你想要的目标**  
    在主机中，使用命令`$ rosrun fv_tracking tracker_kcf`，云台开始转动跟随被锁定的目标。

6. **启动底盘控制节点, 确认云台已经成功跟随需要被跟踪的目标**  
    在docker中，使用命令`$ roslaunch chassis_control chassis_control.launch`, 开启底盘控制节点，输出控制指令。

7. **启动底盘驱动ROS节点**  
    在主机中，使用命令`$ roslaunch pix_driver pix_driver_read.launch`  
    `$ roslaunch pix_driver pix_driver_write.launch`

8. **确定车辆状态，在遥控器上选择MOD为 self-driving模式，如图已经切换到自动驾驶模式，使用MOD按键进行切换**  

   <img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291513665.png" alt="20220114-8bd94d2e" style="zoom: 25%;" />
---

## 硬件介绍

### 云台硬件规格
1. **吊舱云台是根据客户需要进行选型，选择支持变焦（10倍）的云台，具有400万有效像素，**
    **三自由度，最大航向角为+-150度无极旋转，工作电流为240mA(@12V)，重量400g（含相机）。**
2. **云台内部有两路视频流，一路1080P 30FPS本地H.264压缩，存储在设备内，另一路输**
    **出1080P 60FPS格式的HDMI信号，用于无线图传，支持PWM和串口控制。**
3. **云台可以自动对焦，对焦时间小于1s，焦距范围为 F = 4.9 ~ 49mm，105dB宽动态范**
    **围，温度工作范围-10 至 55 摄氏度。**
<center class="half">
    <img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291515746.png" alt="20220114-e0917cd4" width="200" height="200"/><img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291518515.png" alt="20220112-4b9a80b5" width="200" height="200"/>
</center>


---
## 算法框架介绍
**云台跟踪联动算法分为两大部分，第一部分是基于深度学习的目标检测算法和目标跟踪算法，
第二部分是基于PID调节的云台控制算法和底盘控制算法。**  

**流程图如下：**   

<img src="https://www.synotech.top:5523/uploads/2023/04/29/202304291525682.png" style="zoom: 50%;" />

**淡蓝色部分为检测跟踪算法的流程，深蓝色部分为云台和底盘联动控制算法部分**

---

## 云台ROS驱动介绍
**云台ROS驱动，根据云台供应商给的串口指令集进行串口编码控制，在源码中serial_ros包主要用来
将云台的控制指令通过ROS进行转换为串口的指令下发给云台控制板。其中主要需要启动的ROS节点是
serial_node 和 moni。**

启动方式为：  
`$ rosrun serial_ros serial_node`  
`$ rosrun serial_ros moni`

**同时云台驱动中也包含了相机启动和KCF跟踪的fv_tracking包，主要启动的ROS节点为
web_cam和tracker_kcf**。

启动方式为：  
`$ rosrun fv_tracking web_cam`  
`$rosrun fv_tracking tracker_kcf`

**启动后，相机画面及跟踪窗口效果如下：**  

![20220116-67901f3c](https://www.synotech.top:5523/uploads/2023/04/29/202304291526605.png)


---

## 检测算法介绍
**使用的目标检测算法为YOLOX，YOLOX的github[链接](https://github.com/Megvii-BaseDetection/YOLOX "YOLOX")**  
**将YOLOX的源码整合为ROS代码下，改为yolox_ros的ROS包**

启动方式为：  
`$roslaunch yolox_ros yolox_ros.launch`  
**启动之后，yolox_ros节点将会订阅来自云台相机的原始图像数据，生成检测结果并展示在桌面上。**

**yolo_ros**包中发布的话题为：**"/yolox/bounding_boxes"** 和 **" /yolox/image_raw"**。  
**yolo_ros**包中订阅的话题为：**"/camera/rgb/image_raw"**。

**启动后，检测算法窗口效果如下：**  

![20220116-76d60e91](https://www.synotech.top:5523/uploads/2023/04/29/202304291526682.png)


---

## 跟踪算法介绍
**使用的目标跟踪算法SiamFC++，SiamFC++的github[链接](https://github.com/MegviiDetection/video_analyst "SiamFC++")**。
**基于SiamFC++的源码整合为ROS代码下，开发了siamfc_ros的ROS包**。

启动方式为：  
`$roslaunch siamfc_ros siamfc_ros.launch`  
**启动之后，siamfc_ros节点将会订阅来自云台相机的原始图像数据，以及来自yolox_ros节**  
**点的检测到的物体选框，物体选框当前为自动选则模式，选择被检测到的第一个物体，下一步迭代**  
**会添加，优化一个ID选择器，让用户可以选择当前画面中被检测到物体的ID，然后输入到跟踪**  
**器中，会生成跟踪结果并展示在桌面上。**

**siamfc_ros**包中发布的话题为：**"/siamfc/image_raw"** 和 **"/siamfc/bounding_box"**。  
**siamfc_ros**包中订阅的话题为：**"/yolox/bounding_boxes "** 和 **"/camera/rgb/image_raw "**。

**启动后，跟踪算法窗口效果如下：**  

![20220116-3b5f0f20](https://www.synotech.top:5523/uploads/2023/04/29/202304291527559.png)

---

## 联动控制算法介绍
**联动控制算法主要流程是依赖云台的PID调节控制输出的yaw值和底盘的yaw值进行关联，当**  
**云台跟随目标进行转动的时候，将云台的yaw值实时发出到ROS话题**"/gimbal_camera/rpy_data"**上，然后底盘根据云台的**  
**yaw值进行处理，转换为底盘的航向角。**

### 云台控制算法
**云台控制算法采用的PID进行调节，PID控制的error主要是根据图像中心十字靶心像素坐标和被跟踪物体的boundingbox的物体中心的像素坐标的误差。**



### 底盘控制算法
**底盘的控制算法，其中一部分基于视觉的大致观测距离判断进行线速度的控制，另一部分基于云台的yaw值
实时对角速度进行控制。**

---
