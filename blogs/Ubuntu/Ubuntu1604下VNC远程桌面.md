# Ubuntu1604下VNC远程桌面
## 首先，设置Ubuntu的远程控制
设置如下：

![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200729/image_20200729_01.png)

## 安装vncserver
```bash
sudo apt-get install xrdp vnc4server xbase-clients
```

## 取消权限限制
首先，安装dconf-editor
```bash
sudo apt-get install dconf-editor
```
打开dconf-editor，依次展开org->gnome->desktop->remote-access，然后取消 “requlre-encryption”的勾选，如下图：

![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200729/image_20200729_02.png)

## 连接
- 首先，确保两设备之间网络通畅。
- 安装VNC Viewer，打开[下载地址](https://www.realvnc.com/en/connect/download/viewer/)，选择合适的版本下载安装。
- 安装完成后，打开VNC Viewer，新建一个连接，输入目标主机的IP和第一步中设置的密码即可。

![](https://github.com/QuartzYan/QuartzNote/raw/master/images/20200729/image_20200729_03.png)
