---
title: Pytorch深度学习框架-save-load
categories:
  - Technology
  - AI
tag:
  - AI
  - 知识记录
  - Pytorch
date: 2025-04-02 09:30:00
comments: true
template: main.html
---

## 前言
在将模型训练完成之后，下一步自然是要将模型保存下来。自然的，有保存就会有加载。

## 保存模型
保存模型有两种方式，一种是只保存模型的权重，另一种则是将模型的类等信息一并保存。

仅保存模型的权重的方式如下所示：
```py
import torch
import torchvision.models as models

model = models.vgg16(weights='IMAGENET1K_V1')
torch.save(model.state_dict(), 'model_weights.pth')
```

与之相对的，则为：
```py
torch.save(model, 'model.pth')
```

## 加载模型
与保存模型相对应的，加载模型也有两种方式，按照保存模型方式的不同，将会有两种不同的加载方式。

加载模型权重时，必须要先创建一个同一模型的实例：
```py
model = models.vgg16() # we do not specify ``weights``, i.e. create untrained model
model.load_state_dict(torch.load('model_weights.pth', weights_only=True))
model.eval()
```

{% notel blue Note %}
加载模型之后，进行推理前，必须将模型切换到推理模式。
{% endnotel %}

而如果模型在保存的时候采用了第二种方式，加载的时候就可以直接加载，不需要创建同一模型的实例。
```py
model = torch.load('model.pth', weights_only=False),
```

## 后记
在完成了上述的所有内容之后，基本上就涵盖了训练一个模型所需的大部分步骤，快试试训练一个自己的模型吧！
