---
title: 使用GDB调试
categories:
  - Technology
  - Debug
tag:
  - Debug
  - 环境配置
  - C
date: 2025-04-30 15:20:00
comments: true
template: main.html
---
## 前言
程序天天写，Bug改不完，掌握Debug的方式还是非常的有用的，而GDB作为一种调试工具在Linux下这种需要使用命令行执行的情况下还是非常有用的。

## 下载
有时候，Linux的某些系统是自带GDB的，可以利用如下命令检查是否已经安装了GDB
```ps
gdb --version
```
如果输出了版本号就说明已经安装GDB了。

如果系统中没有GDB，可以使用Linux的包管理工具进行下载，具体取决与当前的系统，一些可以参考的安装命令如下所示：
```ps
#使用yum进行安装
sudo yum install gdb
```
```ps
#使用apt安装
sudo apt install gdb
```
```ps
#使用dnf进行安装
sudo dnf install gdb
```

一般情况下Windows下会提供图形化的工具，因此GDB被提起的比较少，但是在Windows下安装GDB也是可行的，例如可以使用Scoop进行安装：
```ps
scoop install gdb
```
scoop的配置教程见：[Scoop环境配置记录](https://blog.cflmy.cn/2024/11/13/Technology/Scoop/Scoop/)

当然如果不想通过Scoop安装也是可以的，可以通过MinGW64进行下载安装，这里可以参考之前的C语言环境配置的教程，步骤几乎完全一样：
[C语言环境配置（最直接）](https://blog.cflmy.cn/2025/04/11/Technology/C/C_Environment/)

## 使用
GDB主要是通过命令行进行调试的，可以对C、C++、Go、java、php等多种语言进行调试。使用GDB的上手难度相对比较高，不过没有关系，慢慢尝试即可。
如果遇到使用上的问题可以使用如下命令获取帮助信息：
```ps
gdb -h
#或者可以写为：
#gdb -help
```

当然仅仅依靠这个帮助信息还是很难完全的掌握GDB的使用方式，下面是作者尝试性的总结了一些内容。

1. 开始调试：
```ps
gdb ./executable_file
#executable_file是需要进行调试的文件编译生成的可执行文件
```
2. 设置断点：
```ps
b breakpoint_information
#breakpoint_information指定了断点的信息，比如行号，函数位置等
```
```ps
info b
#查看断点的信息
```
```ps
d breakpoint_count
#删除编号为breakpoint_count的断点，需要注意这里的breakpoint_count不是行号
#如果要删除所有的断点，则使用命令：
#d breakpoints
```
```ps
enable b breakpoint_count
#启用编号为breakpoint_count的断点
#启用所有的断点则为：
#enable b
```
```ps
disable b breakpoint_count
#禁用编号为breakpoint_count的断点
#禁用所有的断点则为：
#disable b
```
3. 开启运行：
```ps
r
```

4. 显示信息：
```ps
p information
#information一般为你想要查看的信息，如变量的值等，需要注意的是这里的p只会打印一次
```
```ps
display information
#和上文不同，这里的信息在第一次执行后，会在后续每次停下来之后都会再展示一次。
```
```ps
undisplay information
#这里是取消跟踪，以后这些信息就不会再追踪了。
```
```ps
bt
#查看函数调用的信息
```
```ps
l line_count
#打印某一行的代码，line_count表示了行号
```

5. 继续执行
```ps
c
#继续执行直到下一个断点
```
```ps
n
#逐过程执行
```
```ps
s
#逐语句执行
```

6. 修改变量
```ps
set value_name = value
#value表示的是想要指定的变量的值,value_name表示变量名
```

7.执行函数直到退出
```ps
finish
#在函数内部进行执行，直到函数执行结束自动停止。
```

8. 退出GDB
```ps
quit
```

## 后记
通过上述的命令就基本可以进行程序的调试和运行了，后续会举例进行说明的。