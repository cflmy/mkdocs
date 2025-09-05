---
title: Frp工具的安装使用
date: 2025-09-05 10:15:00
comments: true
template: main.html
---

## 前言
在很多的时候，内网中的某台设备是不能直接在公网上被访问的，在这个时候frp就是一个非常好的内网穿透工具。

## 安装
打开[frp发行版](https://github.com/fatedier/frp/releases)找到当前系统对应的架构的发行版。
点击安装下载。

或者可以使用Linux下的wget命令：
```shell
wget href
#href指的是你要下载的frp发行版对应的链接
#例如下面的示例
#wget https://github.com/fatedier/frp/releases/download/v0.64.0/frp_0.64.0_linux_amd64.tar.gz
```

在将压缩包下载之后，需要进行解压缩的操作，windows下的解压操作可以直接借助文件管理系统实现，
而linux下也十分简便：

```shell
rar -zxvf filename
#filename 表示的是要解压的文件名称
#例如下面的示例
#tar -zxvf frp_0.64.0_linux_amd64.tar.gz
```

> 注：在解压缩完成后，frp的安装包一般会显示出压缩包名减去后缀的名称，建议重命名为frp。
接下来进入frp对应的目录即可。

## 配置

打开解压缩后的目录会看到对应的配置文件，在相对比较旧的版本中，frp使用的是json文件进行配置的。
较新版本的frp则使用的是一种名为.toml的文件进行配置的，其实本质都是一样的，下面的教程针对的是toml文件的配置
如果你下载的frp版本较早，则可以参考进行json文件的配置。

> 注：配置和设置中的内容应该将注释删除掉，不然会报错，这里的注释只是为了便于解释配置的原因。

### 服务端的配置文件
```toml
bindAddr = "0.0.0.0"
bindPort = 7000
auth.method = "token"
auth.token = "your token"


webServer.addr = "127.0.0.1"
webServer.port = 7001
webServer.user = "user name"
webServer.password = "user password"
```

### 客户端的配置文件
```toml
serverAddr = "server ip"
serverPort = 7000
auth.method = "token"
auth.token = "token"

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 22
[[proxies]]
name = "http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 80
remotePort = 80

#……
#可以添加其他更多的配置端口
```

## 设置
在进行了上述的设置之后，还需要手动的开启，为了方便开启frp，设置开机自启以及监测状态等，
可以按照下面的方式进行操作。

### 服务端
编辑文件：
```shell
sudo vim /etc/systemd/system/frps.service
```
填入下面的内容并保存：
```service
[Unit]
Description=Frp Server
After=network.target  # 表示此服务需要在网络服务启动后启动

[Service]
User=frps_user      # 运行 frps 的用户，建议使用一个非 root 用户，例如 'nobody' 或你自己的用户
WorkingDirectory=/path/to/frp/frp_XXX  # frps 可执行文件
ExecStart=/path/to/frp/frps -c /path/to/frp/frps.toml # frps 可执行文件的完整路径以及配置文件路径
Restart=on-failure  # 当服务退出时，如果失败则重启
RestartSec=5        # 重启间隔（秒）

[Install]
WantedBy=multi-user.target # 表示此服务应该在多用户运行级别下被启动
```

配置完成后使用下面的命令完成设置：
```shell
sudo systemctl daemon-reload #更新配置信息
sudo systemctl start frps #启动服务端
sudo systemctl enable frps # 设置开机启动
sudo systemctl status frps # 查看运行状态
```

### 客户端
```shell
sudo vim /etc/systemd/system/frpc.service
```
填入下面的内容并保存：
```service
[Unit]
Description=Frp Client
After=network-online.target 
Wants=network-online.target 
Requires=network.target 

[Service]
Type=simple
ExecStart=/path/to/frp/frpc -c /path/to/frp/frpc.toml
Restart=on-failure
RestartSec=5s  

[Install]
WantedBy=multi-user.target
```

配置完成后使用下面的命令完成设置：
```shell
sudo systemctl daemon-reload 
sudo systemctl start frpc
sudo systemctl enable frpc
sudo systemctl status frpc
```

## 后记
到这里之后frp这个好用的内网穿透工具就配置完成了。