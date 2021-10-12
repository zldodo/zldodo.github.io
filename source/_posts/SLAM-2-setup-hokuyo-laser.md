---
title: Hokuyo laser安装与配置
date: 2017-01-15 23:31:14
tags:
- SLAM
- Cartographer
categories: 探知
---
1. 安装  
    Indigo以上的ROS版本需要使用urg_node进行配置 
    首先安装urg_node package
```
sudo apt-get install ros-kinetic-urg-node
```
2. 给Hokuyo供电，并与电脑连接
3. 配置Hokuyo  
    确保你的urg_node能连接到Hokuyo laser scanner  
    索取Hokuyo的权限
```
ls -l /dev/ttyACM0
```
    你将看见跟下面相似的内容
```
crw-rw-XX- 1 root dialout 166, 0 2009-10-27 14:18 /dev/ttyACM0
```
    如果XX是rw，已成功配置。
    如果XX是--: 配置失败，你需要输入
```
sudo chmod a+rw /dev/ttyACM0
```
4. 运行  
    在一个新的终端，输入
```
roscore
```
    在一个新的终端，运行urg_node
```
rosrun urg_node urg_node
```
    你将看到跟下面相似的内容
```
[ INFO] [1487840172.986999317]: Connected to serial device with intensity and ID: H1309434
[ INFO] [1487840173.246729503]: Streaming data.
```
5. 可视化数据  
    在新的终端输入
```
rviz
```
    将Fixed Frame里的map改为laser
    然后添加laserscan，并将其中Topic的空白选项改为/scan