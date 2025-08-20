---
title: Nginx使用
date: 2025-08-20 14:00:00
comments: true
template: main.html
---

## 前言
web开发服务器有很多选择，Nginx作为其中之一，作者本人还是经常使用的，最近刚好需要重新配置Nginx
因此决定写一个使用的教程。

## 安装与启动
1. 更新软件包依赖(可选)
```shell
sudo apt update
```

2. 安装nginx
```shell
sudo apt install nginx
```

3. 启动nginx
```shell
sudo systemctl start nginx
```

4. 设置开机自启动
```shell
sudo systemctl enable nginx
```

5. 检测nginx运行状态
```shell
sudo systemctl enable nginx
```

接下来，只需要打开`本地IP:80`即可查看到nginx的欢迎页面。

> 注：使用yum安装包的安装命令
```shell
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl enable nginx
```

## 配置
进行nginx的配置需要先找到nginx的配置文件位置，该文件一般位于`/etc/nginx/nginx.conf`
文件中，当然也可以使用如下命令找到配置文件的位置：
```shell
nginx -t
```
该命令会检测配置文件是否正确，并输出配置文件的路径信息。
因此该命令可以在进行端口转发、ssl证书加密等配置之后检测配置是否正确。
