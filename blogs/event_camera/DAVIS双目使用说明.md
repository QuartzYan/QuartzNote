# DAVIS双目使用说明

## 1.install dv-gui
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


## 2.install davis ROS driver

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

## 3.install davis stereo driver
- 1.克隆存储库
```bash
cd ~/catkin_ws/src
git clone https://github.com/QuartzYan/davis_stereo_stitch.git #该软件包用于构建DAVIS双目驱动
git clone https://github.com/QuartzYan/davis_obj_detection.git #该软件包用于构建DAVIS动态物体检测的demo
```

- 2.编译
```bash
cd ~/catkin_ws
catkin build davis_stereo_stitch 
catkin build davis_obj_detection 
```

## 4.How to use
### 4.1.调节光圈及焦距
- 1.首先运行下面的命令，打开双目相机 
```bash
roslaunch davis_stereo_stitch davis_stereo_driver.launch
```
- 2.调节两个相机镜头的**光圈和焦距**，使之尽可能一致
- 3.调节完成后，拧紧好两个镜头的紧定螺钉，使**光圈和焦距**不要随意变化
### 4.2.标定相机
- 1.首先运行下面的命令，启动标定程序
```bash
roslaunch davis_stereo_stitch davis_stereo_calibration.launch
```
- 2.移动标定板，采集图像
![运行截图](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200406/image_20200406_03.png)

- 3.采集到一定数量的图像后（不少于30张），点击Start Calibration开始校正，这个过程会持续一段时间(取决于采集的数量和计算机性能)，待计算完成后，点击Save Calibration保存，标定文件会自动保存在*~/.ros/camera_info*下，待下次在ros下启动相机驱动时，便会自动加载
![运行截图](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200406/image_20200406_04.png)


### 4.3.计算单应矩阵
- 1.首先启动计算程序
```bash
roslaunch davis_stereo_stitch davis_stereo_stitch.launch stitch_find_mode:="True"
```
- 2.打开另外一个终端，运行下面的命令，计算一次单应矩阵
```bash
rostopic pub /find_stitch_once std_msgs/Bool "data: true"
```
- 3.在计算程序的终端中会输出计算完的结果，把计算出的矩阵复制到*davis_stereo_stitch\launch\davis_stereo_stitch.launch*文件的第24行：如下图
![截图](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200608/image_20200608_01.PNG)

### 4.4.运行双目拼接驱动
```bash
roslaunch davis_stereo_stitch davis_stereo_stitch.launch
```

### 4.5.运行动态物体侦测
```bash
roslaunch davis_stereo_stitch davis_obj_detection.launch
```