---
title: Mkdocs的使用
date: 2025-08-20 14:00:00
comments: true
template: main.html
---
## 前言
博客的建设有许多的框架，作者本人就尝试使用过docsify和hexo等工具建立过自己的博客，
但是hexo博客在博文增多之后明显能感觉到渲染速度的巨幅减慢，为了让博客的访问速度变快，
作者决定增加一些更多的尝试，也就是Mkdocs。

## 下载安装
安装mkdocs需要配置python环境，可以参考之前的博文，有提到Python环境的配置方法，这里暂且略去。

安装好python环境之后，就可以进行mkdocs的安装了，在终端输入如下命令：
```shell
pip install mkdocs
```

检验mkdocs安装成功可以输入下列命令：
```shell
mkdocs --version
```

## 创建新项目
使用下面的命令可以轻松的创建mkdocs项目
```shell
mkdocs new your_project_name
#注：其中的your_projext_name应当替换为个人的项目名称
#例如，作者创建的命令如下所示：
#mkdocs new mkdocs
```

接下来需要进入创建的项目目录：
```shell
cd your_project_name
```
使用如下命令即可开启一个本地可访问的站点：
```shell
mkdocs serve
```

## 站点的配置
在创建的网站根目录的下方有一个名为`mkdocs.yml`的文件，该文件存储了mkdocs的配置信息，
因此要进行配置需要修改该文件中的内容，详细的配置步骤可见：[Mkodcs官方用户指南](https://mkdocs.org.cn/user-guide/)
有较为详细的配置方式，这里不再赘述。

## 生成静态文件
不同与docsify，mkdocs允许用户将mkdown生成静态的文件用于部署到服务器、github page或者其他的位置。
生成静态文件的方式如下：
```shell
mkdocs build
```

## 进行部署
在生成静态文件之后，只需要将文件放在一些提供静态资源浏览的位置即可将内容展现给更多的读者。

部署到github page上也是同样的思路。
首先需要将本地仓库和github远程仓库进行同步，git操作详见之前的博文，这里不再赘述。

一键部署命令如下所示：
```shell
mkdocs gh-deploy
```

## 后记
在完成了上述操作之后，即可使用mkdocs生成个人博客了。