---
title: Pytorch深度学习框架-transform
categories:
  - Technology
  - AI
tag:
  - AI
  - 知识记录
  - Pytorch
date: 2025-03-26 14:00:00
---
## 前言
为了使数据适合模型进行训练，需要先对数据进行一些变换。在之前的内容中，其实已经出现过transform了，这里对其做一个相对详细的介绍。

## 基础
之前就提到过`torchvision.transforms`模块提供了常用转换。`transform`和`target_transform`

例如，可以看到下面的例子：
```py
import torch
from torchvision import datasets
from torchvision.transforms import ToTensor, Lambda

ds = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor(),
    target_transform=Lambda(lambda y: torch.zeros(10, dtype=torch.float).scatter_(0, torch.tensor(y), value=1))
)
```

这里出现了不是那么熟悉的`ToTensor()`、`Lambda()`和`.scatter_`他们的作用如下：

* `transforms.ToTensor()`：将 `PIL` 图像或 `NumPy` 数组转换为 `Tensor`，并将像素值归一化到 [0, 1] 的范围内。
* `Lambda` 转换应用任何用户定义的 `lambda` 函数。
* `scatter_`函数是一个非常有用的工具，它允许你根据索引将数据分散到张量中。

{% notel blue Note %}
`lambda` 函数和`scatter_`函数应当是非常有用的，在这里简单的写下这么一句话，应当是很难理解的，作者也是一样，理解的非常浅薄。

暂且在这里做一个标记，理解加深之后，需要重新解释这里。
{% endnotel %}

## 更多的用法
* 组合变换（Compose）：
`transforms.Compose(transforms_list)`：将多个变换组合在一起，按照顺序执行。
    
* 调整大小（Resize）：
`transforms.Resize(size)`：调整图像到给定的大小。

* 中心裁剪（CenterCrop）：
`transforms.CenterCrop(size)`：对图像进行中心裁剪，保留中心区域。

* 随机裁剪（RandomCrop）：
`transforms.RandomCrop(size)`：随机裁剪图像的某个区域。

* 翻转（Flip）：
`transforms.RandomHorizontalFlip()`：以一定概率对图像进行水平翻转。
`transforms.RandomVerticalFlip()`：以一定概率对图像进行垂直翻转。

* 归一化（Normalize）：
`transforms.Normalize(mean, std)`：对图像进行归一化操作，将每个通道的值进行标准化。

* 色彩调整（Color Jitter）：
`transforms.ColorJitter(brightness=..., contrast=..., saturation=..., hue=...)`：随机调整亮度、对比度、饱和度和色调。

对于上面的几个操作，下面有一个例子：
```py
import torchvision.transforms as transforms
from PIL import Image

# 创建一个图像转换的组合
transform = transforms.Compose([
    transforms.Resize((256, 256)),
    transforms.RandomHorizontalFlip(),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.5, 0.5, 0.5], std=[0.5, 0.5, 0.5]),
])

# 加载图像
image = Image.open('path_to_image.jpg')

# 应用转换
transformed_image = transform(image)

# transformed_image 现在是一个 Tensor，可以用于模型输入
```

## 后记
Pytorch初学者官方文档这里描述的，作者也是自学理解的也有点差，查阅了一些其他的资料，但是还处在一个比较迷惑的状态，这里后续会更新的。

---
### 同系列
[Pytorch深度学习框架-insatll](https://blog.cflmy.cn/2025/03/18/Technology/AI/Pytorch-install/)
[Pytorch深度学习框架-tensor](https://blog.cflmy.cn/2025/03/18/Technology/AI/Pytorch-tensor/)
[Pytorch深度学习框架-data](https://blog.cflmy.cn/2025/03/20/Technology/AI/Pytorch-data/)
[Pytorch深度学习框架-transform](https://blog.cflmy.cn/2025/03/26/Technology/AI/Pytorch-transform/)
[Pytorch深度学习框架-build](https://blog.cflmy.cn/2025/03/28/Technology/AI/Pytorch-build/)
[Pytorch深度学习框架-train](https://blog.cflmy.cn/2025/04/01/Technology/AI/Pytorch-train/)
[Pytorch深度学习框架-save-load](https://blog.cflmy.cn/2025/04/02/Technology/AI/Pytorch-save-load/)