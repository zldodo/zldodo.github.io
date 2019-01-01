---
title: Cartographer安装与配置
date: 2017-01-14 23:13:31
tags:
- SLAM
- Cartographer
categories: 探知
---
1. 安装依赖项
```
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build
```

2. 创建一个catkin 工作空间
```
mkdir catkin_ws
cd catkin_ws
wstool init src
```
    在创建catkin工作空间前，可先确定自己的环境变量是否设置正确
```
export | grep ROS
```
    若出现如下的，说明是正确的
```
declare -x ROSLISP_PACKAGE_DIRECTORIES=""
declare -x ROS_DISTRO="indigo"
declare -x ROS_ETC_DIR="/opt/ros/indigo/etc/ros"
declare -x ROS_MASTER_URI="http://localhost:11311"
declare -x ROS_PACKAGE_PATH="/opt/ros/indigo/share:/opt/ros/indigo/stacks"
declare -x ROS_ROOT="/opt/ros/indigo/share/ros"
```

3. 下载安装文件
```
wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartographer_ros.rosinstall
wstool update -t src
```
    cere-solver无法下载，手动复制到catkin_ws/src目录下。
4. 安装依赖文件
```
rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
```
    运行(初始化rosdep时）
```sudo rosdep init 
   rosdep update
```
    若出现`ERROR: default sources list file already exists`
    则先输入`sudo rm /etc/ros/rosdep/sources.list.d/20-default.list`
    再输入`sudo rosdep init`
    然后它会提示你输入`rosdep update`
5. 安装与配置
```
catkin_make_isolated --install --use-ninja
source install_isolated/setup.bash
```
6. 测试
    下载后放到``${HOME}/Downloads/``目录下即可用下面的命令使用。
```
# Download the 2D backpack example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag
# Launch the 2D backpack demo.
roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
# Download the 3D backpack example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_3d/b3-2016-04-05-14-14-00.bag
# Launch the 3D backpack demo.
roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=${HOME}/Downloads/b3-2016-04-05-14-14-00.bag
# Download the Revo LDS example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/revo_lds/cartographer_paper_revo_lds.bag
# Launch the Revo LDS demo.
roslaunch cartographer_ros demo_revo_lds.launch bag_filename:=${HOME}/Downloads/cartographer_paper_revo_lds.bag
# Download the PR2 example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/pr2/2011-09-15-08-32-46.bag
# Launch the PR2 demo.
roslaunch cartographer_ros demo_pr2.launch bag_filename:=${HOME}/Downloads/2011-09-15-08-32-46.bag# Download the Taurob Tracker example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/taurob_tracker/taurob_tracker_simulation.bag
# Launch the Taurob Tracker demo.
roslaunch cartographer_ros demo_taurob_tracker.launch bag_filename:=${HOME}/Downloads/taurob_tracker_simulation.bag
```