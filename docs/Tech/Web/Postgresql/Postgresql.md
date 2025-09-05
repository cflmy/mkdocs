---
title: Postgresql安装
date: 2025-08-27 16:00:00
comments: true
template: main.html
---
## 前言
出于一些开发的需要，作者需要安装配置postgresql，这里做个简单的安装记录

## 安装
```shell
sudo apt update
sudo apt install postgresql postgresql-contrib
```

安装完成后，postgresql会自动启动，因此可以使用下列命令验证是否安装并启动成功：
```shell
sudo systemctl status postgresql
```

如果已经成功安装但并未自动启动，请在终端输入：
```shell
sudo systemctl start postgresql
```

## 初始化
在最开始的时候postgresql会初始化一个postgres的初始用户，而这个用户的密码是未设置的，因此需要进行设置：
```shell
sudo -i -u postgres
psql
ALTER USER postgres PASSWORD 'your_password';
\q
```

依次执行上述命令即可进行初始化。

## 后记
安装postgresql的过程还是非常的简单的，读者可以自行尝试。