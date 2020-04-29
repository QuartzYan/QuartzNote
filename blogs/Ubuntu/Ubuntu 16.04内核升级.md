# Ubuntu 16.04内核升级

## 查看当前的内核版本
在终端输入：
```bash
uname -sr
```
输出：
```bash
Linux 4.15.0-29-generic
```
说明当前内核版本为4.15。

## 下载内核
打开[Ubuntu内核下载网站](https://kernel.ubuntu.com/~kernel-ppa/mainline/)，找到你需要的内核版本。
![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_03.png)

点击进入页面，根据自己的硬件设备，选择版本，下载下面四个deb文件。

> [linux-headers-5.2.0-050200_5.2.0-050200.201907231526_all.deb](https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-headers-5.2.0-050200_5.2.0-050200.201907231526_all.deb)  
> [linux-headers-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb](https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-headers-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb)  
> [linux-image-unsigned-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb](https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-image-unsigned-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb)  
 > [linux-modules-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb](https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-modules-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb)  

![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_04.png)

当然，你也可以使用命令行下载：
```bash
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-headers-5.2.0-050200_5.2.0-050200.201907231526_all.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-headers-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-image-unsigned-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2/linux-modules-5.2.0-050200-generic_5.2.0-050200.201907231526_amd64.deb
```
## 升级libssl版本
安装新内核需要依赖libssl1.1，而在Ubuntu16.04中没有libssl1.1的源，故需要手动升级。
查看当前libssl的版本：
```bash

```
下载libssl1.1并安装
```bash
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```

## 安装内核
将上面下载好的内核文件，放到同一个文件夹下，然后运行：
```bash
sudo dpkg -i *.deb
```
## 重启
安装好内核后，重启使之生效：
```bash
sudo reboot
```
重启完成后，再次查看，内核版本：
```bash
uname -sr
```
输出：
```bash
Linux 5.2.0-050200-generic
```
升级成功。

## 删除旧版内核（可选）
查看当前系统安装的内核：
```bash
dpkg --get-selections| grep linux
```
![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_05.png)

卸载旧版内核：
```bash
sudo apt-get remove --purge linux-headers-4.15.0*
sudo apt-get remove --purge linux-image-4.15.0*
sudo apt-get remove --purge linux-modules-4.15.0*
```

## 清理临时文件
```bash
sudo apt-get autoclean 
sudo apt-get autoremove
```
