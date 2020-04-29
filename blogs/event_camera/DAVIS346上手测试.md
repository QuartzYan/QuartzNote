# DAVIS346事件相机上手测试

## 事件相机简介

pass


## Ubuntu下的dv-gui
官方提供了Windows，Ubuntu，MacOS等多平台的支持，这里我们使用的是Ubuntu 16.04的环境。下面是安装过程：

- 1.添加PPA存储库以获取程序及依赖项，打开一个终端，运行下面的命令
 ```bash
 sudo add-apt-repository ppa:ubuntu-toolchain-r/test 
 sudo add-apt-repository ppa:lkoppel/opencv 
 sudo add-apt-repository ppa:janisozaur/cmake-update
 sudo add-apt-repository ppa:inivation-ppa/inivation-xenial
 ```

- 2.更新软件包索引
```bash
sudo apt-get update
```
- 3.安装dv-gui
```bash
sudo apt-get install dv-gui
```
- 4.(可选)如果打算自己做开发，可以安装开发包
```bash
sudo apt-get install dv-runtime-dev
```
- 5.连接好事件相机，运行dv-gui
```bash
dv-gui
```
![dv-gui运行截图](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200315/Screenshot_20200315_01.png)


## ROS下的使用

ROS下我们使用的是[rpg_dvs_ros](https://github.com/uzh-rpg/rpg_dvs_ros)包，下面是它的安装与使用过程：
- 1.安装依赖项
```bash
sudo apt-get install libusb-1.0-0-dev
sudo apt-get install ros-kinetic-camera-info-manager
sudo apt-get install ros-kinetic-image-view
```
- 2.安装编译工具
```bash
sudo apt-get install python-catkin-tools
```
- 3.创建工作区
```bash
mkdir -p ~/catkin_ws/src
cd catkin_ws
catkin config --init --mkdirs --extend /opt/ros/kinetic --merge-devel --cmake-args -DCMAKE_BUILD_TYPE=Release
```
- 4.克隆存储库
```bash
cd ~/catkin_ws/src
git clone https://github.com/catkin/catkin_simple.git #该软件包用于构建DVS/DAVIS驱动程序
git clone https://github.com/uzh-rpg/rpg_dvs_ros.git
```
- 5.编译
```bash
cd ~/catkin_ws
catkin build dvs_ros_driver #如果使用的是DVS，
catkin build davis_ros_driver #如果使用的是DAVIS
catkin build dvs_renderer #编译渲染器
catkin build dvs_calibration dvs_calibration_gui #编译校准工具
```
或者可以运行`$ catkin build`一键编译所有软件包。
- 6.添加串口udev规则
```bash
cd ~/catkin_ws/src/rpg_dvs_ros/libcaer_catkin #进入libcaer_catkin文件夹
sudo ./install.sh #运行安装脚本
```
- 6.连接好事件相机，运行
```bash
source ~/catkin_ws/devel/setup.bash
roslaunch dvs_renderer dvs_mono.launch #如果使用的是DVS
roslaunch dvs_renderer davis_mono.launch #如果使用的是DAVIS
```
![ROS运行截图](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200315/Screenshot_20200315_02.png)

## 基于事件的角点检测
该算法基于的论文[Fast Event-based Corner Detection](http://rpg.ifi.uzh.ch/docs/BMVC17_Mueggler.pdf)，其实现了基于事件的快速角点检测。[代码地址](https://github.com/uzh-rpg/rpg_corner_events)，下面是运行过程：
- 1.安装依赖项
```bash
sudo apt-get install libeigen3-dev
```
- 2.克隆存储库
```bash
cd ~/catkin_ws/src
git clone https://github.com/uzh-rpg/rpg_corner_events.git
```

- 3.修改
由于我们使用的是DAVIS346，其像素为346 x 260，故要修改*distinct_queue.h*,，*fast_detector.h* 和 *harris_detector.h* 中的 **sensor_width_** 和 **sensor_height_**，如下图：

![修改代码片段](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200315/Screenshot_20200315_03.png)

- 4.编译
```bash
cd ~/catkin_ws/src/rpg_corner_events/corner_event_detector
catkin build --this
```

- 5.运行
```bash
source ~/catkin_ws/devel/setup.bash
roslaunch corner_event_detector davis_live.launch
```
![运行截图](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200315/Screenshot_20200315_04.png)



## 附录
---
[1].[事件相机资源整理](https://github.com/uzh-rpg/event-based_vision_resources)
[2].[事件相机开发文档](https://inivation.gitlab.io/dv/dv-docs/docs/getting-started.html)
