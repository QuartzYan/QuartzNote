# Ubuntu下切换默认python版本
由于ROS的默认python版本为2.7，而现在很多代码都是跑在python3.*的，故需要时不时的转换python版本，我们这里使用update-alternatives来为整个系统更改Python版本，以下是操作过程。

## 1.列出所有可用的python替代版本信息
```bash
update-alternatives --list python
```
这里出现错误信息的话，表示update-alternatives还没有添加Python的替代版本。

## 2.添加Python的替代版本
```bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1  # 添加2.7的版本

sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2  # 添加3.5的版本，这里根据你实际安装的版本添加
```

## 3.列出所有可用的python版本信息
```bash
update-alternatives --list python
```
这时就可以看到刚才添加的python版本了。

## 4.切换默认python版本
```bash
sudo update-alternatives --config python
```
输入想要切换的版本代号（1或2），回车就可以了。