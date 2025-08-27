---
title: Mysql安装
date: 2025-08-27 16:00:00
comments: true
template: main.html
---
## 前言
数据库的安装配置还是非常重要的，今天介绍数据库的安装配置

## 安装
```shell
sudo apt install mysql-server
```

安装后输入:
```shell
mysql --version
```
若安装成功，则将会输出版本信息

## 启动
```shell
sudo systemctl start mysql
sudo systemctl enable mysql #设置开机自启
sudo systemctl status mysql #查看状态
```

## 设置root密码
```shell
sudo mysqladmin -u root password
```

## 后记
mysql的安装内容大概只有这么多，日后会尝试增补使用方式的。

