---
title: Pytorch深度学习框架-data
categories:
  - Technology
  - AI
tag:
  - AI
  - 知识记录
  - Pytorch
date: 2025-03-20 15:55:00
---
## 前言
> 处理数据样本的代码可能会变得混乱且难以维护；我们理想情况下希望我们的数据集代码与我们的模型训练代码解耦，以获得更好的可读性和模块化。

以上出自Pytorch官方文档的原语，更好的可读性和模块化更有利于程序员进行开发，因此今天我们尝试了解Pytorch框架下数据集和数据处理的一些方式。

## 加载数据集
数据集加载过程中需要指定下列参数：
* `root`-存储数据的路径
* `train`是否为训练集，True表示是训练集，False表示是测试集。
* `download`是否从互联网下载，若数据在root路径下不可用，设置downald为True可以从互联网上下载所需的数据。
* `transform`或者`target_transform`指定了特征和标签转换。transform 和 target_transform，分别用于转换输入和目标。

{% notel red Warn %}
当使用 download=True 创建数据集对象时，文件首先在根目录中下载和解压缩。此下载逻辑不是多进程安全的，因此如果在分布式设置中运行，可能会导致冲突/竞争条件。在分布式模式下，我们建议在设置分布式模式*之前*创建一个虚拟数据集对象来触发下载逻辑。

> 这里作者也不是很理解，暂时先留在这里，留待以后继续解决。

{% endnotel %}

仅从上面的解释还是不够清楚，下面是一个从`TorchVision`加载`Fashion-MNIST`数据集的一个示例：
```py
import torch
from torch.utils.data import Dataset
from torchvision import datasets
from torchvision.transforms import ToTensor

training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor()
)

test_data = datasets.FashionMNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor()
)
```

{% notel green Tip %}
TorchVision 在 torchvision.datasets 模块中提供了许多内置数据集，以及用于构建自定义数据集的实用工具类。TorchVision专门用来处理图像，通常用于计算机视觉领域。

[TorchVison数据集官方文档](https://pytorch.ac.cn/vision/stable/datasets.html)

而Fashion-MNIST 是 Zalando 的文章图像数据集，包含 60,000 个训练示例和 10,000 个测试示例。每个示例包含一个 28×28 灰度图像以及来自 10 个类别之一的相关标签。
{% endnotel %}

## 可视化数据集
使用matplotlib库可以实现数据集的可视化。
```py
import matplotlib.pyplot as plt

#这里的数据集承接上文的加载部分
labels_map = {
    0: "T-Shirt",
    1: "Trouser",
    2: "Pullover",
    3: "Dress",
    4: "Coat",
    5: "Sandal",
    6: "Shirt",
    7: "Sneaker",
    8: "Bag",
    9: "Ankle Boot",
}
figure = plt.figure(figsize=(8, 8))
cols, rows = 3, 3
for i in range(1, cols * rows + 1):
    sample_idx = torch.randint(len(training_data), size=(1,)).item()
    img, label = training_data[sample_idx] #这里是手动索引了数据集，和列表类似
    #可采用的方法为：Datasets：training_data[index]
    figure.add_subplot(rows, cols, i)
    plt.title(labels_map[label])
    plt.axis("off")
    plt.imshow(img.squeeze(), cmap="gray")
plt.show() #将画好的图像显示出来。
```

## 自定义数据集
自定义数据集需要从`torch.utils.data.Dataset`也就是我们在前文导入的`Dataset`继承得到一个新的子类，这个子类必须要实现以下三个函数：
* `__init__()`实例化 Dataset 对象时运行一次,初始化包含数据的目录、注释文件和两个转换
* `__len__()`函数返回数据集中样本的数量。
* `__getitem__()`函数加载并返回给定索引 idx 处数据集的样本。根据索引，它识别磁盘上数据的位置，将数据转换为张量，并在元组中返回张量数据和相应的标签。

下面以FashionMNIST 数据集为例，这些数据图像存储在目录 img_dir 中，它们的标签单独存储在 CSV 文件 annotations_file 中。
其中label.csv文件为如下格式：
```csv
tshirt1.jpg, 0
tshirt2.jpg, 0
......
ankleboot999.jpg, 9
```

此时实现的自定义数据集为：
```py
import os
import pandas as pd
from torchvision.io import read_image

class CustomImageDataset(Dataset):
    def __init__(self, annotations_file, img_dir, transform=None, target_transform=None):
        self.img_labels = pd.read_csv(annotations_file)
        self.img_dir = img_dir
        self.transform = transform
        self.target_transform = target_transform

    def __len__(self):
        return len(self.img_labels)

    def __getitem__(self, idx):
        img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
        image = read_image(img_path)
        label = self.img_labels.iloc[idx, 1]
        if self.transform:
            image = self.transform(image)
        if self.target_transform:
            label = self.target_transform(label)
        return image, label
```

## 小批量传递数据
> Dataset 一次检索我们数据集的特征和标签一个样本。在训练模型时，我们通常希望以“小批量”传递样本，在每个 epoch 重新打乱数据以减少模型过拟合，并使用 Python 的 multiprocessing 来加速数据检索。

为了实现上述的目的，需要使用`torch.utils.data.DataLoader`也就是`DataLoader`。
使用的方式如下：
```py
from torch.utils.data import DataLoader

train_dataloader = DataLoader(training_data, batch_size=64, shuffle=True) #batch_size指定了每批次的大小
test_dataloader = DataLoader(test_data, batch_size=64, shuffle=True) #shuffle指定了是否被打乱。
```

### 迭代DataLoader
DataLoader是一个可迭代的对象，我们可以迭代进行模型训练等操作，如下列示例：
```py
# Display image and label.
train_features, train_labels = next(iter(train_dataloader))
print(f"Feature batch shape: {train_features.size()}")
print(f"Labels batch shape: {train_labels.size()}")
img = train_features[0].squeeze()
label = train_labels[0]
plt.imshow(img, cmap="gray")
plt.show()
print(f"Label: {label}")
```

## 后记
以上就是数据集的一些信息了，如果想要了解更多的内容，可以直接在官方文档寻找。

---
### 同系列
[Pytorch深度学习框架-insatll](https://blog.cflmy.cn/2025/03/18/Technology/AI/Pytorch-install/)
[Pytorch深度学习框架-tensor](https://blog.cflmy.cn/2025/03/18/Technology/AI/Pytorch-tensor/)
[Pytorch深度学习框架-data](https://blog.cflmy.cn/2025/03/20/Technology/AI/Pytorch-data/)
[Pytorch深度学习框架-transform](https://blog.cflmy.cn/2025/03/26/Technology/AI/Pytorch-transform/)
[Pytorch深度学习框架-build](https://blog.cflmy.cn/2025/03/28/Technology/AI/Pytorch-build/)
[Pytorch深度学习框架-train](https://blog.cflmy.cn/2025/04/01/Technology/AI/Pytorch-train/)
[Pytorch深度学习框架-save-load](https://blog.cflmy.cn/2025/04/02/Technology/AI/Pytorch-save-load/)