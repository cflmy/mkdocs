---
title: Latex基础语法
categories:
  - Technology
  - Latex
tag:
  - Latex
  - 知识记录
date: 2025-05-08 15:50:00
comments: true
template: main.html
---
## 前言
之前作者就写过一篇关于[Markdown基础语法](../Markdown/Markdown.md)的记录，Latex作为另一种排版工具自然也不能落下。

本文主要记录Latex的基础语法，但是Latex的下载安装还是相对具有一定的挑战的，读者可查阅本博客的：
[Scoop环境配置记录](../../Tool/Scoop/Scoop.md)

其中有提到如何配置Latex的内容，此外读者还可尝试自行配置TextLive，这里不再赘述。

推荐阅读：[LaTeX 入门](https://oi-wiki.org/tools/latex/)

## 语法
和Markdwon略有不同，Latex的使用相对比较复杂，但与之类似的，能够实现的功能也相应会更加的多。

实际上，作者直到现在最推荐的操作依然是，找一个Latex模板，根据需要把内容填进去就
足够了，当然这样的操作还是需要一些对Latex的了解，因此还是需要掌握一些内容的。
### 注释
Latex的注释是用`%`号引导的：
```latex
%这是一个Latex注释
```

### 准备部分
这一部分是为了编译一份Latex所必要的一些内容，因为书写完这里的内容之后，你会"惊喜"的发现，
Latex仍然是空白的，你不会得到任何编译的结果，因此我把这一部分成为准备部分。

#### 指定文档类型
一般在文档的开始，需要指定Latex文档的类型，这是因为使用Latex可以完成诸如论文、书籍、幻灯片等
众多文档内容的编写，因此需要在文件的开头指定你想要写一份什么文档：
```latex
\documentclass[option]{documetn class you want to use}
%[]括号内的内容是可选的，一般可以用来指定仅凭文档类型不能指定的内容，如纸张大小等
%{}括号内的内容是必须的，指定了想要使用的文档类型，一般中文的有ctexbook、ctexart和ctexbeamer
%例如，一个示例为：
%\documentclass[12pt, a4paper]{ctexart}
%设置了文档类型为ctexart，A4纸，字体大小为12pt
```
这里需要特别注意的是，不同的文档类型可能会对Latex有不同的支持，有些语法可能在当前文档类型下不支持，导致编译失败，因此指定文档类型是一件非常重要的事情

#### 导入宏包
宏包，其实可以理解为写程序的时候导入的库，这些库中包含了一些方便使用的函数，宏包也是类似的
其中包含了一些可能会用到的命令。
```latex
\usepackage{package name}
%{}内的内容指定了宏包的名称
```

#### 导言
Latex的导言和Makdown的元信息看上去非常的相似，不过略有不同的是，Latex的导言是可以被显示出来的，
一般都是文档的标题作者等。

常见的导言有：
```latex
\title{Your Title}
\date{Date}
\author{Author}
%这些都是常见的导言。
```

### 正文
到了正文部分，Latex就会开始产生输出了，由于文档类型的不同，使用不同的文档类型，配合不同的命令可能会产生千差万别的
内容。好在，有些内容是通用的，可以让我们迅速的掌握。

#### 结束与开始
正文的开始与结束需要使用命令显式的指定：
```latex
%开始正文
\begin{document}

    Hello World!
    %"Hello World!"将会被渲染输出，你可以这样检查Latex是否配置成功
    %需要注意的是，Latex对缩进不敏感(除了包含代码的时候)
    %这里作者为了表示一定的逻辑关系，采用了缩进
\end{document}
%结束正文
```

#### 特殊的字体
有时候正文的不同部分需要使用不同的字体，常见的修改方式如下：
```latex
\textup{Hello World!} %直立
\textit{Hello World!} %意大利
\textsl{Hello World!} %倾斜
\textsc{Hello World!} %小型大写
\textbf{Hello World!} %加宽加粗
```

#### 导言区的渲染
准备部分写的导言自然不是无用的，使用以下命令可以让输出导言中包含的内容：
```latex
\begin{document}
    \maketitle
    %会将导言的内容输出
    Hello World!

\end{document}
```

#### 章节结构
一般情况下，为了使文档看上去更有条理，需要进行章节结构的划分：
> 注：下面的内容一般实在ctexart下使用的，其他文档类型可能会不支持。
> 如无特别说明，下面的一些内容，一般都会在ctexart文档类型下适用

常用的章节结构划分命令为：
```latex
\begin{document}
    \chapter{章节一}
        \section{标题一}
            \subsection{次级标题一}
                Hello World!

\end{document}
```

#### 目录
有了章节目录的划分，就可以自动的生成目录：
```latex
\begin{document}
    \tableofcontents
    %会自动的生成目录
    \chapter{章节一}
        \section{标题一}
            \subsection{次级标题一}
                Hello World!

\end{document}
```
需要注意的是，部分编辑器在使用该命令之后，需要编译两次。

#### 图片
插入图片需要导入宏包`graphicx`，因此完整的插入方式为：
```latex
\begin{figure}[option]
%[]方括号中的内容是可选的，指定了位置参数，也就是你希望把图表放在哪里，很多时候
%如果遇到了图片排版问题，效果不太好，一般就是这里出现了问题，可以使用htbp参数指定为
%选择最优的位置
    \centering
    %设置图片居中，这是比较推荐的选择
    \includegraphics[option]{path to your image}
    %[]方括号中的内容是可选的，指定了图片的一些显式样式，例如宽度高度等
    \caption{description for your image}
    %这个命令会在图片的下方渲染一段描述文字
    \label{image label}
    %label可以用于后续引用图片
\end{figure}
```

#### 表格
表格的插入和图片有一些类似之处，具体的命令为：
```latex
\begin{table}[option]
    \centering
    \caption{description for your table}
    \begin{tabular}{option}
        content & ……
    \end{tabular}
    %tabular中的内容指定了表格的样式，option指定了表格的形状，行和列有多少
    %中间的内容使用&分割切换到下一行需要使用\显式的指定
\end{table}
```

表格的生成在latex中略显麻烦，可以使用工具完成：
[Tables Generator](https://www.tablesgenerator.com/)

#### 列表
列表主要包含有序列表和无序列表
##### 有序列表
```latex
\begin{enumerate}
    \item 有序列表一
    \item 有序列表二
    \item 有序列表三
\end{enumerate}
```
##### 无序列表
```latex
\begin{itemize}
    \item 无序列表一
    \item 无序列表二
    \item 无序列表三
\end{itemize}
```

#### 公式
公式的插入分为行内公式和行间公式：
> 注：导入公式一般也需要导入宏包，不过数学公式支持的宏包比较多，取决于你的选择，因此这里没有列出
##### 行内公式
```latex
$ math $
```
##### 行间公式
行间公式的书写方式比较多：
* 不带标号的公式:
```latex
$$
    math
$$
%或者可以使用这种方式
\[
    math
\]
```

* 带标号的公式
```latex
\begin{equation}
    math
\end{equation}
%或者使用下面的命令生成一组带有标号的公式
\begin{eqnarray}
    math1 \\
    math2
\end{eqnarray}
```

#### 代码
代码的插入也是需要导入宏包的，一般而言可以导入宏包：
`listings`和`xcolor`（该宏包主要提供代码高亮的支持）

使用的方式为：
```latex
\begin{lstlisting}
    Code
\end{lstlisting}
```

此外还可以通过下面的命令显式的改变代码高亮的显示方式：
```latex
\lstset{
    rules1,
    rules2,
    ……
}
```

此外还可以通过外部文件输入，这一点还是非常好用的，这意味我们可以不再需要进行大段代码的复制，
直接从源文件导入代码：
```latex
\lstinputlisting[option]{path to your code file}
%[]中是一些可选项，指定了一些显示选项
```

## 后记
Latex的基本操作就是这样了，当然Latex的许多操作还是非常深奥的，所以还是作者的一贯看法——
找个模板，修改为自己的内容就可以了。
