---
title: Pmail邮件服务器配置记录
date: 2025-09-11 16:50:00
comments: true
template: main.html
---
## 前言
由于作者进行smtp收发信件的需求，需要自行搭建一个邮件服务器，但是一般的邮件服务器内存
占用比较大，就在这是Pmail进入了作者的视角，正如作者遇到的问题一样，这个服务器的开发作者也遇到了同样的问题，
因此开发了这个轻量的邮件服务器，刚巧网上对于这个邮件服务器的配置教程较少，作者这里做一个配置记录。

## 下载安装
在这里作者推荐的配置方式是使用docker，不过docker部署实际上会增大一部分资源消耗，尽管这个邮件服务器已经十分的轻量级了
，作者还是想要折腾一下，选择了直接部署的方式。

首先需要从发行版安装对应的版本:
[Pmail发行版](https://github.com/Jinnrry/PMail/releases)
安装之后，根据不同的版本解压到本地即可。

> 注:为了方便配置，建议你在解压后将文件夹中的可执行文件名称改为pmail，后续的配置都会按照这个名称进行操作。

安装之后，进入安装目录，linux需要为pmail添加可执行的权限
```shell
sudo chmod +x pmail
```

之后使用下面的方式启动pmail
```shell
./pmail
```

> 注：pmail在开启的时候会自动的使用80端口但是这个端口在使用的时候经常会被占用，因此可以使用下面的命令
从其他端口启动pmail：
```shell
./pmail -p your_chosen_port
```
只不过这样子做只能手动的进行证书的配置，因此作者推荐暂时性的将运行在80端口上的进程结束，
例如关闭nginx服务等。
当然你也可以使用下面的命令查看运行在80端口上的服务:
```shell
sudo lsof -i :80
```
接着根据显示的进程号，按照下面的操作杀死进程即可。
```shell
sudo kill <PID>
```

启动pmail之后，按照提示在浏览器输入你的服务器的ip进入进行配置即可，如果你的pmail是在80端口开启的
那么直接选择自动部署ssl证书即可。

## 配置
上述是安装步骤，但是正如注释中提到的，很多时候80端口是需要使用的，因此我们需要修改pmail的运行端口
打开你的pamil安装路径下的`config/confgi.json`文件，找到`"httpPort":0`,将这个配置信息修改为你想要Pmail运行的端口。
接着重新启动即可控制pmail不再占用80端口。

## nginx设置反向代理和证书配置
在进行了上一步的操作之后，再次打开你会发现证书失效了，这时候可以使用nginx设置反向代里结合certbot工具完成端口转发和证书配置

可以参考本博客内容:
[Nginx](../Nginx/Nginx.md)
[Certbot](../Certbot/Certbot.md)
完成配置

## 设置开机自启等
接下来我们还需要设置Pmail的开机自启工作，在命令行输入:
```shell
sudo vim /etc/systemd/system/pmail.service
```
接着在这个文件中输入下面的配置即可：
```
[Unit]
Description=Pmail
After=network-online.target 
Wants=network-online.target 
Requires=network.target 

[Service]
Type=simple
ExecStart=/path/to/pmail/pmail
Restart=on-failure
RestartSec=5s  

[Install]
WantedBy=multi-user.target
```

之后就可以利用下面的方式进行管理了
```shell
sudo systemctl daemon-reload #更新配置信息
sudo systemctl start pmail #启动pmail
sudo systemctl enable pmail # 设置开机启动
sudo systemctl status pmail # 查看运行状态
```