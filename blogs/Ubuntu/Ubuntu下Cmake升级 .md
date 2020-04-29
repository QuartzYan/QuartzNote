# Ubuntu下Cmake升级
## 查看当前cmake版本
首先，查看当前cmake的版本：
```bash
cmake --version
```
输出：
```bash
cmake version 3.5.1
CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
可以看出当前版本为3.5.1

## 下载新版本
打开[cmake压缩包下载网站](https://cmake.org/files/)，选择版本：

![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_06.png)

点击进如后，根据需要，选择不同版本，点击下载：
![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_07.png)


当然也可以使用命令行下载：
```bash
wget https://cmake.org/files/v3.15/cmake-3.15.0-Linux-x86_64.tar.gz
```

## 安装
首先，解压下载好的文件：
```bash
wget https://cmake.org/files/v3.15/cmake-3.15.0-Linux-x86_64.tar.gz
```
将解压后的文件夹复制到*/opt*路径下：
```bash
tar -zxvf ./cmake-3.15.0-Linux-x86_64.tar.gz
sudo cp -r ./cmake-3.15.0-Linux-x86_64 /opt/cmake-3.15.0
```
建立软链接
```bash
sudo ln -sf /opt/cmake-3.15.0/bin/*  /usr/bin/
```
再次查看cmake的版本：
```bash
cmake --version
```
输出：
```bash
cmake version 3.15.0
CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
可以看出已经升级到了3.15.0，完成。
