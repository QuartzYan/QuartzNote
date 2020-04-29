# Ubuntu下各类代理设置
## 1.终端走代理的几种方法
```bash
#1.http，https代理
export http_proxy="http://127.0.0.1:10808"
export https_proxy="http://127.0.0.1:10808"

#2.socket5协议
export http_proxy="socks5://127.0.0.1:10808"
export https_proxy="socks5://127.0.0.1:10808"

#3.全局
export ALL_PROXY="socks5://127.0.0.1:10808"
```

## 2.git走代理的设置
```bash
#1.设置代理
git config --global http.proxy 'socks5://127.0.0.1:10808' 
git config --global https.proxy 'socks5://127.0.0.1:10808'
#2.取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 3.v2-ray的安装与使用
```bash


```
