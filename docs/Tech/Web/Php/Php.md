---
title: Php安装
date: 2025-08-27 16:00:00
comments: true
template: main.html
---
## 前言
php作为后端语言，以轻量级，简单著称，就算后端服务使用java，c++等提供，有时候也会用到php。
此外有相当一部分的CMS(内容管理系统)都是采用php语言完成的。还有一些非常好用的网络工具，也需要php的使用。

## 安装
```shell
sudo apt update
sudo apt install php
```

安装特定版本的php(使用ondrej/php PPA进行特定版本的获取)
```shell
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.2
```

安装一些常用的插件:
* php-mysql: 用于连接 MySQL 数据库。
* php-gd: 用于图像处理。
* php-xml: 用于 XML 解析。
* php-mbstring: 用于多字节字符串处理。
* php-intl: 用于国际化和本地化。
* php-zip: 用于处理 zip 文件。
* php-curl: 用于发送 HTTP 请求。
* php-fpm: PHP FastCGI Process Manager，通常与 Nginx 配合使用。
```shell
sudo apt install php-mysql php-gd php-xml php-mbstring php-intl php-zip php-curl php-fpm
```

检验安装是否成功
```shell
php -v
```
如果安装成功，将会输出php的版本信息

## 配置

找到php的配置文件位置：
```shell
php --ini
```
接下来进行配置即可，注意配置后重启即可。

## 后记
php就安装到这里了，php不是重点，毕竟php开发有非常多的开源工具，只需要配个环境即可。