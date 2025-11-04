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
sudo systemctl status nginx
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

在初次打开nginx的配置文件时，nginx会有一个初始的配置信息：
```conf
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
	# multi_accept on;
}

http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	types_hash_max_size 2048;
	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;

	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}


#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
#
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}
#}

```

这个配置信息在不同的版本有不同的内容，但大致上的功能是一致的，通过修改这里的内容就可以完成一些更多的操作。

### 设置端口转发
在观察配置信息之后，我们会发现中间有一行为`include /etc/nginx/sites-enabled/*;`
打开观察后下面应该有一个default的文件，其中存放着配置信息，因此我个人推荐在该文件夹下创建配置文件从而避免污染原有的内容。
例如你可以在`/etc/nginx/sites-enabled/`创建一个名为`testconfig`的文件
输入下面的内容:
```
server {
    listen 80;  # 监听 80 端口 (HTTP)
    server_name testconfig.example.com;  # 你的域名或服务器的 IP 地址（可选）

    location / {
        proxy_pass http://127.0.0.1:8080;  # 将请求转发到本地 8080 端口
        proxy_set_header Host $host;  # 转发原始 Host 头部
        proxy_set_header X-Real-IP $remote_addr;  # 转发客户端真实 IP
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 转发客户端 IP (包括代理IP)
        proxy_set_header X-Forwarded-Proto $scheme;  # 转发协议 (http 或 https)
    }
}
```

如果要让端口转发支持Websocket则需要在location这个位置添加配置如下:
```
		proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
```

这样你就可以完成一个端口转发任务。

## 后记
nginx的作用还是很大的，读者可以自行尝试其他如负载均衡之类的配置。
