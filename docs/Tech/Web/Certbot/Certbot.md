---
title: Certbot-ssl证书管理
categories:
  - Technology
  - Certbot
tag:
  - Certbot
date: 2025-09-05 15:50:00
comments: true
template: main.html
---
## 前言
ssl证书是建立一个网站的必备一环，不然在浏览器访问你的网站的时候就非常容易弹出来一个不安全的提示，为了避免这个问题，
就需要使用ssl证书，而Let's Encrypt就刚好提供了免费的ssl证书，为了简化配置，我们可以使用certbot来辅助我们完成证书的配置。

## 安装
```shell
sudo apt update
sudp apt install certbot
```

## 使用certbot自动配置证书
作者使用的是nginx服务器因此使用的也是nginx服务器对应的配置证书的方式
```shell
sudo apt install python3-certbot-nginx #安装用于nginx的插件
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

## 后记
使用certbot来完成ssl证书的配置还是非常的方便的，读者可以自行尝试。