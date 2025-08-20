---
title: Pytorch深度学习框架-build
date: 2025-03-28 16:00:00
categories:
  - Technology
  - AI
tag:
  - AI
  - 知识记录
  - Pytorch
comments: true
template: main.html
---
## 前言
在熟悉了上述的这些操作之后，接下来就要来到非常重要的一步，如何构建一个神经网络模型了。

## 构建神经网络
### 获取可用于加速的设备
在张量部分，就提到过，进行运算的时候，可以使用加速器进行计算上的加速，因此在即将进行模型的训练之前，我们可以先找到当前环境下是否存在可以用来进行加速的加速器。
例如可以执行下列操作：
```py
device = torch.accelerator.current_accelerator().type if torch.accelerator.is_available() else "cpu"
print(f"Using {device} device")
```

### 定义神经网络
在获得可用于加速的设备之后，接下来就可以定义神经网络了，类似与之前的自定义数据集，在Pytorch中，自定义的神经网络也需要通过子类继承的方式来完成。
其中需要继承的类为`torch.nn.module`
例如，我们可以尝试自定义一个神经网络如下：（示例中出现了许多还没有被介绍到的函数，如果感到困惑的话，请忽略函数的具体内容。在这一步，只需要了解如何通过子类继承来定义一个神经网络即可。）
```py
# 环境导入
import os
import torch
from torch import nn
from torch.utils.data import DataLoader
from torchvision import datasets, transforms

class CustormNeuralNetwork(nn.Module):
    def __init__(self):
        super().__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10),
        )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits
```

### 创建模型对象
在自定义了神经网络之后，接下来就可以创建对象，如下面的代码就完成了这样一个动作，并把创建的模型展示出来。
```py
# 这里的device是之前创建的加速器，这里的含义就是将创建的模型对象挪到加速器上，或者可以直接表述为在加速器上创建模型。
model = NeuralNetwork().to(device)
print(model)
```

## 后记
在Pytorch的官方文档上，后面还有许多的内容，例如我们构建模型的时候用到了什么东西，为什么要这么做，大多数都是nn模块的东西，但是作者个人认为，这样介绍的不是很清楚，单说创建模型，到这一步就已经足够了，不同的神经网络自然有不同的构建方式。当然，这里可能是作者目前认知过于浅薄，没有认识到必要性，因此在后续，或许会有所更改和增补的。
