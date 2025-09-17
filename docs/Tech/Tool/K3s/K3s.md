---
title: k3s使用
date: 2025-09-16 14:20:00
comments: true
template: main.html
---
## 前言
K3s 是一个轻量级的 Kubernetes 发行版，它针对边缘计算、物联网等场景进行了高度优化。
对于k8s（Kubernetes）的简称，想要部署的内存占用和节点占用还是比较高的，因此这个轻量
的替代品，对于作者而言还是非常具有吸引力的。

> 注：安装k3s需要先安装docker，没有安装的可以参阅作者之前的内容进行配置。[docker使用](../Docker/Docker.md)

[k3s文档](https://docs.rancher.cn/)

## 安装
k3s的节点分为master节点和worker节点，因此安装节点的方式也有所区别。
### Mater节点
k3s的安装还是比较的简单的，官方提供了一个Master一键安装脚本：
```shell
curl -sfL https://get.k3s.io | sh -
```

对于国内的用户则有替代的方式为：
```shell
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -
```

> 实际测试的时候，发现上述命令能够正常的安装k3s，但是无法启动pods，在查阅了官方社区的
[使用国内资源安装 K3s 全攻略](https://forums.rancher.cn/t/k3s/1416)，应该在末尾增加镜像指定:
```shell
curl –sfL \
     https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | \
     INSTALL_K3S_MIRROR=cn sh -s - \
     --system-default-registry "registry.cn-hangzhou.aliyuncs.com"
```

> 注：在这里的中文参考手册中，应当配置拉取其他镜像的国内加速站，但是作者尝试按照教程配置
后发现并没有生效，似乎是k3s更新版本后，在使用上述指定镜像仓库安装的时候，会自动替换国内镜像，
在查阅资料后，作者暂时也不清楚是否还需要进行国内镜像的配置，因此这里不再给出配置经过，
如果后续出现使用上的问题，再进行配置。

> 注：如果master节点的配置较低，不建议安装很多的插件，可能会导致出现服务器卡死等问题。
因为作者就出现了，卡死的现象。

在安装完master节点后,可以使用下面的命令检测是否安装成功：
```shell
sudo systemctl status k3s
```

在检测安装成功后，我们需要得到node-token的值，让我们在后续能够成功进行work节点的配置：
```shell
sudo cat /var/lib/rancher/k3s/server/node-token
```

### Worker节点
依然使用一键安装脚本不同的是，需要传入我们在上一步得到的node-token的值:
```shell
sudo curl -sfL https://get.k3s.io | K3S_URL=https://master-ip:6443 K3S_TOKEN=node-token sh -
#master-ip为master节点的ip地址
#node-token需要输入我们在上一步得到的值
```

国内节点则应该替换为:
```shell
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | K3S_URL=https://master-ip:6443 K3S_TOKEN=node-token INSTALL_K3S_MIRROR=cn sh -
```

在执行完成上述命令之后，可以在master节点使用下面的命令查看是否已经成功的启动：
```shell
sudo kubectl get nodes
```
如果显示出各个节点的主机名，则说明配置已经成功。

## 后记
作者在配置k3s的时候，发现自己的服务器动不动就挂了，虽然k3s消耗已经很低了，但是想要实现一些比较高级的功能还是
需要一些增加配置，就会导致内存消耗增加，服务器就很容易挂掉，在这种情况下，作者无奈只能服从了，
裸机部署，尽可能的减少资源的消耗。

也正是因为作者的服务器经常出现问题，作者只能保证Master节点的配置是正确的，后续的Worker节点，
读者如果有需要，可以自行搜索教程进行配置。