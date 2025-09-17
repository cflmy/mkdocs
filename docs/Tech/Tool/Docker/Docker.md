---
title: Docker使用
date: 2025-09-16 10:10:00
comments: true
template: main.html
---
## 前言
作者做自己的应用在相当长的一段时间都比较的排斥使用docker，这是因为作者的云服务器
配置太低了，docker部署会消耗多一些资源，虽然多消耗的资源并不多，但是作者会更倾向于裸机部署，
不过现在，作者的云服务器资源逐渐增多，多一些资源消耗，简化配置变得更加有吸引力，
此外docker的使用也是使用k8s等的基础，因此作者决定尝试折腾一下，使用docker。

[docker中文文档](https://docker.cadn.net.cn/)

## 安装
安装docker的方式比较多种多样，作者仅参考[菜鸟教程Ubuntu手动安装教程](https://www.runoob.com/docker/ubuntu-docker-install.html)
，尝试进行了docker的安装，其他的教程中，似乎非常容易因为网络连接等问题出现安装失败，
这个教程中使用了ustc的镜像，相对比较稳定。

首先，需要卸载旧版本的docker：
```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```
接着按照下列的命令进行配置：
```shell
sudo apt-get update
```
```shell
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
```shell
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

接下来，在完成安装之后，可以运行下面的命令来验证是否安装完成：
```shell
docker --version #若成功输出版本信息则进行下一步
sudo docker run hello-world
```

> 上述验证安装是否安装成功的命令可能会执行失败，原因是刚安装好的docker本地仓库中没有hello-wolrd
的本地镜像，需要远程拉取，而远程镜像库在国内可能会被阻拦，因此如果要稳定的获取镜像，
建议按照下面的步骤进行配置：
```shell
sudo vim /etc/docker/daemon.json
```
接着在配置文件中输入如下配置信息：
```json
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://hub-mirror.c.163.com"
    ]
}
```
保存退出后需要重启docker来完成配置的更新：
```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```
完成上述步骤后，在执行验证命令应该能够正常的完成任务。

## 使用
### Dockerfile
使用docker进行构建，需要先进行Dockerfile的书写，建议直接参阅[Dockerfile概述](https://docker.cadn.net.cn/manuals/build_concepts_dockerfile)，按照相关文档的说明来进行书写。

### 构建和运行
在进行Dockerfile的书写之后，就可以进行docker构建和运行了，可以使用下面的方式进行构建：
```shell
sudo docker build -f path/to/Dockerfile -t name:tag .
#其中path/to/Dockerfile为你书写的Dockerfile的相对路径，如果就在当前目录下，则可以省略
#name为你构建的容器的名称，tag则为对应的版本
#有时候，在书写完Dockerfile之后，进行构建可能会提示找不到Dockerfile，在排查拼写，权限等错误之后，如果还不能找到
#解决方案，建议重启终端。
```

在完成构建之后，只需要在终端执行命令：
```shell
docker run -p 127.0.0.1:inner_port:outer_port name:tag
#注意运行的时候请将port和后面的name:tag进行修改
```

## 后记
暂时作者就先折腾到这里好了，后续如果发现其他有意思的玩法，会继续更新的，此外作者配置docker
还有一个目的是配置k3s，关于这个工具的配置放在其他文件下了，感兴趣的可以参阅。