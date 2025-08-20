---
title: Pytorch深度学习框架-tensor
categories:
  - Technology
  - AI
tag:
  - AI
  - 知识记录
  - Pytorch
date: 2025-03-18 14:50:00
comments: true
template: main.html
---
## 前言
继续学习Pytorch，这一部分针对模型训练的时候需要用到的重要数据结构——张量部分进行汇总。

## 张量是什么？
> 在数学中，张量是一种用于描述多维空间中的线性关系的对象。它可以看作是向量和矩阵的推广。张量的英文是tensor。

这是一句听起来就非常复杂的描述，很符合数学的严谨特征，但是会让人无形之中不知所云。简单的理解一些，其实张量就是一种描述事务的数据类型。

由于描述数据会有许多不同的方面，比如衡量一张图片，长、宽、高、色彩等都可以用来描述这张图片，张量可以简单的视为是这些数据的合集，我们把这些数据有规律的放在一起，就形成了张量。

在深度学习中，很多时候输入和输出都要从很多个维度来考虑，在这个时候，张量就非常的有用了。

所以，暂时性的，我们知道这样几件事就足够了
* 张量是一堆有规律的数据。
* 张量对深度学习很有用。
* 张量描述了事物的不同方面，也就是张量的维度。

## 初始化
暂时知道了上述内容，接下来我们就可以学习Pytorch如何创建张量并使用了。
### 从数据创建
```py
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
```

### 从numpy创建
```py
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
```

### 从张量（旧的）创建
```py
x_ones = torch.ones_like(x_data) #这里的两个张量保持不变

x_rand = torch.rand_like(x_data, dtype=torch.float) #这里张量的数据类型发生了变化
```

### 随机值或常量值创建
```py
shape = (2,3,) #定义张量的维度
rand_tensor = torch.rand(shape) #从随机数创建（注意数据都是0-1的）
ones_tensor = torch.ones(shape) #张量全为1
zeros_tensor = torch.zeros(shape) #张量的数据全为0
```

## 张量的属性
张量的属性描述了形状、数据类型以及存储设备这些信息，获取张量属性的方式如下：
### 形状
张量的形状就是它的维度，也就是我们最开始提到的，张量衡量了事物的多少个方面
```py
tensor = torch.rand(3,4) #随便创建一个张量

print(f"Shape of tensor: {tensor.shape}")#打印了形状
```
### 数据类型
张量的数据类型就是构成张量的每个数据的类型，比方说图片长512，是个整数，这个数据类型就是整型，对应的小数称为浮点型，详细的数据类型可以查阅相关资料。
```py
print(f"Datatype of tensor: {tensor.dtype}")#打印数据类型
```
### 存储设备
张量是数据，数据的存储位置在那个设备上，存储设备就是那个。
```py
print(f"Device tensor is stored on: {tensor.device}")#打印存储设备
```

## 张量的运算
张量存储了数据，这些数据被我们通过一些处理训练得到模型，这种情况下，如何进行张量的运算是一个非常有必要的事情。

### 运算的加速
默认情况下，张量在 CPU 上创建。需要使用 .to 方法显式地将张量移动到加速器（在检查加速器可用性之后）。但是需要特别注意的是，跨设备复制大型张量在时间和内存方面可能非常昂贵。

可以借鉴的使用加速器的方式如下所示：
```py
#先检查加速器可用性，如果可用，就将张量移动到加速器
if torch.accelerator.is_available():
    tensor = tensor.to(torch.accelerator.current_accelerator())
```

### 索引和切片
这里的操作和numpy中的很多操作十分相似，不了解的，可以查阅numpy得到这些操作的含义。
```py
tensor = torch.ones(4, 4)
print(f"First row: {tensor[0]}")
print(f"First column: {tensor[:, 0]}")
print(f"Last column: {tensor[..., -1]}")
tensor[:,1] = 0
print(tensor)
```

### 张量的连接
```py
t1 = torch.cat([tensor, tensor, tensor], dim=1)
print(t1)
```

### 算数运算
```py
# This computes the matrix multiplication between two tensors. y1, y2, y3 will have the same value
# ``tensor.T`` returns the transpose of a tensor
# ``tensor.T``返回张量的转置
y1 = tensor @ tensor.T
y2 = tensor.matmul(tensor.T)

y3 = torch.rand_like(y1)
torch.matmul(tensor, tensor.T, out=y3)


# This computes the element-wise product. z1, z2, z3 will have the same value
# 这里计算了张量的逐元素乘积
z1 = tensor * tensor
z2 = tensor.mul(tensor)

z3 = torch.rand_like(tensor)
torch.mul(tensor, tensor, out=z3)
```

### 张量聚合
将量的所有值聚合为一个值
```py
agg = tensor.sum()
agg_item = agg.item()
print(agg_item, type(agg_item))
```

### 就地运算
将结果存储到操作数中的运算称为就地运算。它们用 _ 后缀表示。例如：x.copy_(y), x.t_() 将更改 x
```py
print(f"{tensor} \n")
tensor.add_(5)
print(tensor)
```
{% notel red Warn %}
就地运算可以节省一些内存，但在计算导数时可能会出现问题，因为会立即丢失历史记录。因此，不鼓励使用它们。
{% endnotel %}

## 张量与NumPy
CPU 上的张量和 NumPy 数组可以共享它们的底层内存位置，更改一个会更改另一个。
### 张量到NumPy
```py
t = torch.ones(5)
n = t.numpy()
```

### NumPy到张量
```py
n = np.ones(5)
t = torch.from_numpy(n)
```

{% notel blue Note %}
需要注意的是，这里互相转换的时候的本质是共享内存位置，因此对一方进行更改，会对应改变另一个
{% endnotel %}

## 后记
以上就是张量的基本信息了，如果想要了解更多的内容，可以直接在官方文档寻找。
