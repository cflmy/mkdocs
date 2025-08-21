---
title: WordPress安装与使用
date: 2025-08-21 14:30:00
author: Wang Jiaxiang
comments: true
template: main.html
---

## 前言
全球有40%的网站都是使用WordPress制作而成的，因此作者决定尝试配置并使用WordPress。

## 下载安装
```shell
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
```
在执行完上述步骤后，安装的目录下将会出现一个名为wordpress的目录

## 配置数据库
在进行wordpress安装之后，还应当创建一个与其关联的数据库和数据库用户，在后续使用。
wordpress官网提供了许多的配置方式，这些配置都需要安装额外的工具，因此这里作者给出一种使用sql语句进行配置的方式
```sql
-- 1. 创建用户
CREATE USER newuser WITH PASSWORD 'password';

-- 2. 创建数据库 
CREATE DATABASE mydatabase OWNER newuser; -- 数据库所有者设为新用户。 如果数据库已存在，可跳过该步骤

-- 3. 授予权限
GRANT CREATE, SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO newuser; -- 授予对公共模式中的所有表的操作权限。  更精细的控制，替换 "ALL TABLES" 为表名列表
```

## 安装
安装worpress其实是一个非常简单的步骤，但是在实际操作的时候会遇到一定的问题。

比方说将wordpress上传到网站目录下，这就是一个非常有意思的操作。

> 你可能会有这样的疑问，我不是要使用wordpress进行网站的开发，怎么还要让我将wordpress
上传到网站的服务呢？

实际上再仔细观察wordpree之后，我们会发现，它的主要构成是php文件，而php文件是要运行在服务器环境下的
这是我们需要将wordpress上传到网站服务器目录下的原因。

这里应当澄清的说，wordpress是一个CMS系统(内容管理系统)而非网站服务器，因此并不能独立的运行
在我们的云服务器上打开某个端口自动的进行运行。

在实际操作的过程中，wordpress官网提供了一些方法，作者在这里推荐使用nginx或者apache网站服务器
只需要将wordpress上传到这两个服务器指定的网站根目录即可。

当然，也可以上传到一个子文件夹，接下来只需要在浏览器访问我们使用的网站服务器对应的IP+端口即可进行
wordpress的配置了，由于比较简单，因此这里不再赘述。

## 后记
在配置完wordpress，作者认为作为一个内容管理系统，wordpress绝对是首屈一指的。