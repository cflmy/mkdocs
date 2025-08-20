---
title: Pytorch深度学习框架-train
categories:
  - Technology
  - AI
tag:
  - AI
  - 知识记录
  - Pytorch
date: 2025-04-01 14:18:00
comments: true
template: main.html
---
## 前言
在官方文档中，这一部分应当是有一些前置的内容，但是哪些内容更加偏向于细节，读起来就比较的让人摸不着头脑，因此作者训练部分提前到了前面。

## 模型训练
模型训练是一个迭代的过程，在每一次迭代中，都会通过机器学习的方式对于模型的参数进行优化。最终，就可以得到一个训练出来的结果。

### 超参数
在进行模型训练之前，可以先将超参数定义下来，超参数是为了帮助我们进行控制模型的训练速度。

一般需要定义的超参数有：
* Number of Epochs - 迭代数据集的次数
* Batch Size - 在更新参数之前通过网络传播的数据样本数
* 学习率 - 在每个批次更新模型参数的量。较小的值会导致学习速度变慢，而较大的值可能会导致训练期间出现不可预知的行为。

```py
# 定义超参数
epochs = 5
learning_rate = 1e-3
batch_size = 64
```

### 损失函数和优化算法
在进行模型训练之前，除了预先定义好超参数，还需要指定要使用的损失函数和优化算法

#### 损失函数
损失函数测量获得的结果与目标值的差异程度，这是在训练过程中想要最小化的损失函数。

如可以用如下方式指定损失函数
```py
# Initialize the loss function
loss_fn = nn.CrossEntropyLoss()
```

#### 优化算法
优化是调整模型参数以减少每个训练步骤中的模型误差的过程。优化算法定义了此过程的执行方式。

如可以用如下方式指定优化算法
```py
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)
```

此外，需要特别提到的是，在正式的训练中，优化的步骤为：
* 调用重置模型参数的梯度。默认情况下，梯度累加;为了防止重复计数，我们在每次迭代时都将它们显式归零。`optimizer.zero_grad()`

* 通过调用 .PyTorch 会根据每个参数来存储损失的梯度。`loss.backward()`

* 一旦我们有了梯度，我们就会调用以通过在 backward pass 中收集的梯度来调整参数。`optimizer.step()`

### 迭代训练
正如之前所说的，训练是一个迭代的过程，如，我们可以通过下列方式定义用于训练的函数
```py
def train_loop(dataloader, model, loss_fn, optimizer):
    size = len(dataloader.dataset)
    # Set the model to training mode - important for batch normalization and dropout layers
    # Unnecessary in this situation but added for best practices
    model.train()
    for batch, (X, y) in enumerate(dataloader):
        # Compute prediction and loss
        pred = model(X)
        loss = loss_fn(pred, y)

        # Backpropagation
        loss.backward()
        optimizer.step()
        optimizer.zero_grad()

        if batch % 100 == 0:
            loss, current = loss.item(), batch * batch_size + len(X)
            print(f"loss: {loss:>7f}  [{current:>5d}/{size:>5d}]")


def test_loop(dataloader, model, loss_fn):
    # Set the model to evaluation mode - important for batch normalization and dropout layers
    # Unnecessary in this situation but added for best practices
    model.eval()
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    test_loss, correct = 0, 0

    # Evaluating the model with torch.no_grad() ensures that no gradients are computed during test mode
    # also serves to reduce unnecessary gradient computations and memory usage for tensors with requires_grad=True
    with torch.no_grad():
        for X, y in dataloader:
            pred = model(X)
            test_loss += loss_fn(pred, y).item()
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()

    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n")
```

接下来，调用上述函数，我们就可以优化模型的参数，并跟踪当前的神经网络效果。

```py
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)

epochs = 10
for t in range(epochs):
    print(f"Epoch {t+1}\n-------------------------------")
    train_loop(train_dataloader, model, loss_fn, optimizer)
    test_loop(test_dataloader, model, loss_fn)
print("Done!")
```

### 后记
在了解了这些之后，基本上就可以进行自己的模型的训练了。

