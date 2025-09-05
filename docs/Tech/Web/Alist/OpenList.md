---
title: OpenList-Alist的推荐替代品
categories:
  - Technology
  - Cloud
tag:
  - Cloud
  - 环境配置
  - OpenList
date: 2025-09-05 14:25:00
comments: true
template: main.html
---
## 前言
作者最近有需求重新配置云盘管理工具，但是查阅最新的资料的时候发现原本开源的Alist现在不光闭源了
而且闹出了一些不好的新闻，所幸Alist有一个开源替代版本OpenList,在进行使用之后效果非常好，几乎可以说能够完全替代Alist。

[OpenList官网用户指南链接](https://doc.oplist.org/guide)

## 安装
原有的一件安装脚本OpenList也是支持的，因此可以直接查阅原本的Alist配置教程，官网也有详细的配置方式，这里不再赘述，
但是作者有些时候需要在没有代理加速的环境下配置OpenList,或许也有用户需要在这种环境下完成安装，因此这里给出手动安装的方式。

[OpenList发行版](https://github.com/OpenListTeam/OpenList-APT/releases)

打开发行版链接，找到自己系统对应的版本，进行下载。

下载后将压缩包解压，并进入对应的文件夹即可。
> Linux用户可以使用下面的方式进行解压：
```shell
tar -zxvf filename
```

## 设置
手动安装的OpenList与一键脚本安装的还是有一定的区别的，因此在使用的过程中也有一定的差别，如果你是使用一键安装脚本
安装，可以查阅之前Alist的使用方法，如果是手动安装，则请按照下面的方式继续操作。

首先进入Openlist的安装文件夹，接着在终端输入：
```shell
./openlist server
```
此时openlist会自动的开启，第一次开启的时候还会产生一个初始化的管理员密码，请记得保存。

上一步是使用OpenList的必要一步也是我们验证是否安装成功的一种方式，但是每次都要手动的开启
着实是有些麻烦的，因此可以通过更进一步的操作来帮助我们更好的使用OpenList。

编辑配置文件：
```shell
sudo vim /usr/lib/systemd/system/openlist.service
```

将下面的内容输入进去(注意有些内容需要根据下载的路径修改):
```service
[Unit]
Description=openlist
After=network.target
[Service]
Type=simple
WorkingDirectory=path_openlist
ExecStart=path_openlist/openlist server
Restart=on-failure
[Install]
WantedBy=multi-user.target
```

接下来输入:
```shell
sudo systemctl daemon-reload #更新配置信息
sudo systemctl start openlist #启动服务端
sudo systemctl enable openlist # 设置开机启动
sudo systemctl status openslist # 查看运行状态
```

## 后记
在完成了上述的配置之后便能放心的使用OpenList了，相比于现在已经闭源的Alist，作者还是更加推荐使用OpenList的。