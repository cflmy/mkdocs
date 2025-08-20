---
title: Alist配置域名和SSL
date: 2025-04-16 10:10:00
categories:
  - Technology
  - Cloud
tag:
  - Cloud
  - 环境配置
  - Alist
---
## 前言
之前介绍了，如何配置一个Alist，以及如何进行WebDav挂载到本地，但是读者可能也会发现，每次配置都要进行IP地址的输入，而且没有SSL加密配置也会麻烦许多，安全性也没有保证。

在这种情况下，可以通过配置域名和SSL来进行解决上述问题。

[作者配置的Alist](https://alist.cflmy.cn)

## 环境准备
1. nginx环境，由于系统版本问题，如何配置环境不再赘述。
2. 域名。
3. SSL证书。

## 域名的配置
首先需要找到nginx的配置文件，在命令行中输入：
```shell
nginx -t
```
会输出一些nginx的信息，其中包含了nginx配置文件的信息。

接下来需要进行该配置文件的编辑，可以直接通过系统自带的编辑器，linux环境下还可以在命令行输入：
```shell
vim PathToNginxConfigFile/nginx.conf
```

在打开的窗口进行编辑即可。

打开后里面会包含一些nginx的服务信息，其中的内容，如果不清楚含义尽量不要改变。

在文件中添加Alist的配置内容：
`Todo`表示待修改
```conf
server{
        #配置Alist
        listen 80; //此时监听的是http端口
        server_name your_domain; //Todo 改变your_domain字段，替换为需要配置的域名
        #定义最大上传文件的限制值，可按需求更改
        client_max_body_size 100m;
        location / {
        proxy_pass http://localhost:port; //Todo 改变port字段，替换为你的Alist服务端口，一般默认为5244
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```

## SSL证书的配置
作者在配置这里的过程中，发现网上的教程基本上都是对Alist的Config.json文件实现的，作者尝试之后，发现确实可以成功，但是配置上会麻烦一些。

实质上，仍然可以使用nginx实现SSL证书的配置，只需要对上述配置进行一点修改：
```conf
server{
        #配置Alist443端口
        listen 443 ssl; //此时监听的是https端口
        server_name your_domain; //Todo 改变your_domain字段，替换为需要配置的域名
        #填写证书文件绝对路径
        ssl_certificate PathToCertificate.pem;//Todo 修改证书文件路径
        #填写证书私钥文件绝对路径
        ssl_certificate_key PathToCertificateKey.key;//Todo 修改证书私钥文件路径
        #表示优先使用服务端加密套件。默认开启
        ssl_prefer_server_ciphers on;
        #定义最大上传文件的限制值，可按需求更改
        client_max_body_size 100m;
        location / {
        proxy_pass http://localhost:port; //Todo 改变port字段，替换为你的Alist服务端口，一般默认为5244
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```

完成上述的配置之后，只需要保存并退出。

{% notel green Tip %}
vim进入Insert模式按`i`退出按`Ecs`键，之后输入`:wq`即可保存并推出。
{% endnotel %}

接下来需要测试配置文件是否配置成功，命令行仍然输入：
```shell
nginx -t
```
如果配置成功，重启nginx即可：
```shell
sudo systemctl restart nginx
```

## 后记
总体而言，借助nginx进行Alist服务的配置还是比较简单的，读者可以自行尝试。

---
### 同系列
[Alist云盘管理工具的配置](https://blog.cflmy.cn/2025/03/20/Technology/Cloud/Alist-install/)
[RaiDrive云盘挂载本地工具的使用](https://blog.cflmy.cn/2025/03/20/Technology/Cloud/RaiDrive/)
[NetMount云盘挂载本地工具的使用](https://blog.cflmy.cn/2025/03/25/Technology/Cloud/NetMount/)
[Windows自带功能实现云盘挂载本地](https://blog.cflmy.cn/2025/03/26/Technology/Cloud/Windows/)
[Alist配置域名和SSL](https://blog.cflmy.cn/2025/04/16/Technology/Cloud/Alist-specs/)