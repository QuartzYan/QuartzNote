# AX200 WiFi6网卡驱动安装
## Windows下的安装
AX200目前仅支持win10 64位的系统，首先下载驱动程序，打开[英特尔® Wi-Fi 6 AX200 驱动](https://downloadcenter.intel.com/zh-cn/product/189347/intel-wi-fi-6-ax200)下载页面，下载wifi驱动和蓝牙驱动。
![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_01.png)

下载好后，直接双击安装即可。

## Ubuntu下的安装
### 首先下载驱动
打开[英特尔® 无线适配器的 Linux* 支持](https://www.intel.cn/content/www/cn/zh/support/articles/000005511/network-and-i-o/wireless-networking.html)页面，下载驱动文件，如下图：
![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200404/image_20200404_02.png)
当然，你也可以使用命令行下载：
```bash
wget https://wireless.wiki.kernel.org/_media/en/users/drivers/iwlwifi/iwlwifi-cc-46.3cfab8da.0.tgz
```
### 升级内核
在上面的下载页面可以看出，驱动文件要求的Linux内核版本需要大于5.1，如果你的系统内核版本低于5.1，则需要升级内核。首先查看当前系统内核版本：
```bash
uname -rs
```
如大于5.1则直接进行下一步，若低于5.1请看此文档升级内核。

### 安装驱动
说是安装驱动，其实只要将之前下载的驱动文件复制到*/lib/firmware/*下即可。
首先，解压文件：
```bash
tar -zxvf iwlwifi-cc-46.3cfab8da.0.tgz
```
进入解压后文件夹，然后复制**iwlwifi-cc-a0-46.ucode**到*/lib/firmware/*文件夹：
```bash
cd iwlwifi-cc-46.3cfab8da.0/
sudo cp ./iwlwifi-cc-a0-46.ucode /lib/firmware/
```
之后重启即可完成：
```bash
sudo reboot
```
