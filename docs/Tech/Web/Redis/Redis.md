---
title: Redis安装
date: 2025-08-27 16:00:00
comments: true
template: main.html
---
## 前言
Redis是一个内存数据库，也是一种查询中间层，这使得使用Redis可以进行后端查询服务变得更快速。
掌握Redis有助于进行Web开发。

[Redis中文教程](https://www.redis.net.cn/)
[Redis菜鸟教程](https://www.runoob.com/redis/redis-tutorial.html)

## 下载安装
直接使用包管理工具进行下载即可：
```shell
sudo apt update
sudo apt install redis
```
在安装完成之后，可以使用下面的命令验证安装是否完成：
```shell
redis-server --version
```
若是输出版本信息，则说明安装已经成功。

一般而言使用包管理工具的redis会自动的启动，可以使用下面的命令检测是否正常的启动：
```shell
sudo systemctl status redis
```

## 使用
> 在vscode扩展中可以连接redis服务，拓展名称为mysql可以尝试使用。

这里采用python进行redis的连接操作，首先需要安装python对应的库，由于这里的redis是配置在ubuntu环境下的，
因此python的配置需求也相应的进行一定的变更。

```shell
sudo apt install python3-redis
```

接下来就可以使用下面的操作进行数据库的连接和使用：
```python
from redis import StrictRedis
redis = StrictRedis(host='localhost', port=6379, db=0)
redis.set('test', 'cflmy')
print(redis.get('test'))
```

## 后记
详细的使用redis的教程肯定是不能简单的通过一篇博客介绍完成，因此这里仅介绍简单的安装和使用。
