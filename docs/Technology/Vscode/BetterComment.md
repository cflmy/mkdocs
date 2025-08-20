---
title: Better Comments-Vscode插件配置
categories:
  - Technology
  - Vscode
tag:
  - Vscode
  - 效率提升
  - 生产力工具
date: 2024-11-13 16:29:49
---
## 前言
Vscode中有许多非常好用的插件，能够提升程序员的写作效率。其中Better Comments作为一款注释增强类型的插件，有助于程序员养成良好的代码风格。并且该软件可以通过配置一些新的选项实现更多功能，这里分享一下我的配置。

## 下载安装
Vscode插件商店搜索Better Comments,下载安装第一个即可。
![](../img/BetterComments/1.png)

## 使用
### 初始配置
作者初始提供了几种默认的tag，使用图例如下：
![](../img/BetterComments/2.png)

### 进阶配置
打开settings.json，找到如下处
~~~json
"better-comments.tags": [
    {
        "tag": "!",
        "color": "#FF2D00",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {
        "tag": "?",
        "color": "#3498DB",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {
        "tag": "//",
        "color": "#474747",
        "strikethrough": true,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {
        "tag": "todo",
        "color": "#FF8C00",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {
        "tag": "*",
        "color": "#98C379",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    }
]
~~~
这里是原有的配置，在这里的基础上，在[]之间可以增加一些自己的配置，以下是我个人的配置，可以作为参考。

#### 增加整个文件的注释信息：
##### 配置文件
~~~json
    //extra information
    {//Date 黑底白字
        "tag": "Date",
        "color": "#FFFAFA",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "#1C1C1C",
        "bold": false,
        "italic": false
    },
    {//Time 黑底白字
        "tag": "Time",
        "color": "#FFFAFA",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "#1C1C1C",
        "bold": false,
        "italic": false
    },
    {//Author 黑底白字
        "tag": "Author",
        "color": "#FFFAFA",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "#1C1C1C",
        "bold": false,
        "italic": false
    },
    {//Update 白底(由于显示上的一些小问题，后改成略偏黄的颜色)黑字
        "tag": "Update",
        "color": "#1C1C1C",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "#FFE4B5",
        "bold": false,
        "italic": false
    }
~~~
注意{}后的逗号，如果是在末尾就不要加，不在末尾请注意添加，后文也是类似的
##### 效果展示
![](../img/BetterComments/3.png)

#### 添加对于结构的注释信息
##### 配置文件
~~~json
    //Code structer
    {//Begin 红色加粗
        "tag": "Begin",
        "color": "#FF2D00",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//End 红色加粗
        "tag": "End",
        "color": "#FF2D00",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Def 橙色加粗
        "tag": "Def",
        "color": "#FF8800",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Default 橙色加粗 实际上因为Def已经自动变为橙黄色
        "tag": "Default",
        "color": "#FF8800",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Main 橙色加粗
        "tag": "Main",
        "color": "#FF8800",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Init 黄色加粗
        "tag": "Init",
        "color": "#FFFF33",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Assign 黄色加粗
        "tag": "Assign",
        "color": "#FFFF33",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Branch 绿色加粗
        "tag": "Branch",
        "color": "#33FFFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//If 绿色加粗
        "tag": "If",
        "color": "#33FFFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Case 绿色加粗
        "tag": "Case",
        "color": "#33FFFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Judge 绿色加粗
        "tag": "Judge",
        "color": "#33FFFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Loop 蓝色加粗
        "tag": "Loop",
        "color": "#66CCFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//For 蓝色加粗
        "tag": "For",
        "color": "#66CCFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//While 蓝色加粗
        "tag": "While",
        "color": "#66CCFF",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Call 紫色加粗
        "tag": "Call",
        "color": "#CC00CC",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Include 紫色加粗
        "tag": "Include",
        "color": "#CC00CC",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Class 紫色加粗
        "tag": "Class",
        "color": "#CC00CC",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    },
    {//Import 紫色加粗
        "tag": "Import",
        "color": "#CC00CC",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": true,
        "italic": false
    }
~~~
##### 效果展示
![](../img/BetterComments/4.png)

#### 增加对于新的标签的支持
##### 配置文件
~~~json
    //Label
    {//Error 红色下划线
        "tag": "Error",
        "color": "#FF2D00",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Todo 橙色下划线
        "tag": "todo",
        "color": "#FF8C00",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Debug 黄色下划线
        "tag": "Debug",
        "color": "#FFFF33",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Complete 绿色下划线
        "tag": "Complete",
        "color": "#33FFFF",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Optimize 蓝色下划线
        "tag": "Optimize",
        "color": "#66CCFF",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Tip 蓝色下划线
        "tag": "Tip",
        "color": "#66CCFF",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Warn 紫色下划线
        "tag": "Warn",
        "color": "#CC00CC",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    },
    {//Question 紫色下划线
        "tag": "Question",
        "color": "#CC00CC",
        "strikethrough": false,
        "underline": true,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": false
    }
~~~
需要注意的是，新定义的Todo和之前的有所重复，生效规则是在前的生效，这里是出于统一风格的原因进行了统一。
##### 效果展示
![](../img/BetterComments/5.png)

#### 增加对于一些数字的支持（这里使用了一些莫兰迪色）
##### 配置文件
~~~json
    //使用数字代表一些经典的莫兰迪色，特别增加了斜体，单纯整活
    {//1 莫兰迪红 斜
        "tag": "1",
        "color": "#965454",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//2 莫兰迪橙 斜
        "tag": "2",
        "color": "#d8caaf",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//3 莫兰迪黄 斜
        "tag": "3",
        "color": "#faead3",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//4 莫兰迪绿 斜
        "tag": "4",
        "color": "#b5c4b1",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//5 莫兰迪青 斜
        "tag": "5",
        "color": "#7b8b6f",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//6 莫兰迪蓝 斜
        "tag": "6",
        "color": "#8696a7",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//7 莫兰迪紫 斜
        "tag": "7",
        "color": "#eee5f8",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//8 莫兰迪粉 斜
        "tag": "8",
        "color": "#ead0d1",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    },
    {//9 莫兰迪灰 斜
        "tag": "9",
        "color": "#bfbfbf",
        "strikethrough": false,
        "underline": false,
        "backgroundColor": "transparent",
        "bold": false,
        "italic": true
    }
~~~
##### 效果展示
![](../img/BetterComments/6.png)

这里比较特殊的一点在于如果是多位数，是自动按照最后一位进行色彩的改变的，因此如果你对注释进行编号，每条注释之间的颜色都会发生改变，不会出现111，112，113……之间颜色一直是单调不变的情况

## 后记
总体而言，Better Comments这个插件使用起来还是很棒的，主要原因是这个插件支持的文件类型相对较多。此外还可以在配置中打开highlight PlainText（这个配置是默认关闭的）这样的选择为这个插件提供了更多可能。

---
### 同系列
[Better Comments-Vscode插件配置](https://blog.cflmy.cn/2024/11/13/Technology/Vscode/BetterComment/)
[MarkDown Preview Enhanced自定义样式配置-Vscode插件配置](https://blog.cflmy.cn/2025/03/16/Technology/Vscode/MarkDownPrview/)
[自定义Vscode界面](https://blog.cflmy.cn/2025/04/25/Technology/Vscode/CustomCssandJsloader/)