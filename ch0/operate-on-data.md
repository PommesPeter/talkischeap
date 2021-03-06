# 数据操作

在深度学习中，我们通常会频繁地对数据进行操作。作为动手学深度学习的基础，本节将介绍如何对内存中的数据进行操作。

在`MXNet`中，`NDArray`是一个类，也是存储和变换数据的主要工具。为了简洁，本书常将`NDArray`实例直接称作`NDArray`。如果你之前用过`NumPy`，你会发现`NDArray`和`NumPy`的多维数组非常类似。然而，`NDArray`提供`GPU`计算和自动求梯度等更多功能，这些使`NDArray`更加适合深度学习。

## 1 创建`NDArray`

我们先介绍`NDArray`的最基本功能。**请注意，接下来的操作需要使用`jupyter notebook`或`python`命令行**。请不要也不需要使用`pycharm`等`IDE`新建一个工程来执行这些命令。并且，**在执行这些命令的时候如果关闭了命令行，就需要从头再来**。如果你有一定的python基础，就会知道为什么。

首先从`MXNet`导入`ndarray`模块。这里的`nd`是`ndarray`的缩写形式。

```python
from mxnet import nd
```

然后我们用`arange`函数创建一个行向量。

```python
x = nd.arange(12)
x
```

你会看到这样的输出：

```
[ 0.  1.  2.  3.  4.  5.  6.  7.  8.  9. 10. 11.]
<NDArray 12 @cpu(0)>
```

这时返回了一个`NDArray`实例，其中包含了从0开始的12个连续整数。从打印`x`时显示的属性`<NDArray 12 @cpu(0)>`可以看出，它是长度为12的一维数组，且被创建在CPU使用的内存上。其中“@cpu(0)”里的0没有特别的意义，并不代表特定的核。

我们可以通过`shape`属性来获取`NDArray`实例的形状。

```python
x.shape
```

你可以看到这样的输出：

```
(12,)
```

我们也能够通过`size`属性得到`NDArray`实例中元素（element）的总数。

```python
x.size
```

你可以看到这样的输出：

```
12
```

下面使用`reshape`函数把行向量`x`的形状改为(3, 4)，也就是一个3行4列的矩阵，并记作`X`。除了形状改变之外，`X`中的元素保持不变。

```python
X = x.reshape((3, 4))
X
```

你会看到这样的输出：

```
[[ 0.  1.  2.  3.]
 [ 4.  5.  6.  7.]
 [ 8.  9. 10. 11.]]
<NDArray 3x4 @cpu(0)>
```

注意`X`属性中的形状发生了变化。上面`x.reshape((3, 4))`也可写成`x.reshape((-1, 4))`或`x.reshape((3, -1))`。由于`x`的元素个数是已知的，这里的`-1`是能够通过元素个数和其他维度的大小推断出来的。

接下来，我们创建一个各元素为0，形状为(2, 3, 4)的张量。实际上，之前创建的向量和矩阵都是特殊的张量。

```python
nd.zeros((2, 3, 4))
```

你可以看到这样的输出：

```
[[[0. 0. 0. 0.]
  [0. 0. 0. 0.]
  [0. 0. 0. 0.]]
 [[0. 0. 0. 0.]
  [0. 0. 0. 0.]
  [0. 0. 0. 0.]]]
<NDArray 2x3x4 @cpu(0)>
```

类似地，我们可以创建各元素为1的张量：

```python
nd.ones((3, 4))
```

你可以看到这样的输出：

```
[[1. 1. 1. 1.]
 [1. 1. 1. 1.]
 [1. 1. 1. 1.]]
<NDArray 3x4 @cpu(0)>
```

我们也可以通过Python的列表（list）指定需要创建的`NDArray`中每个元素的值。

```python
Y = nd.array([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
Y
```

你可以看到这样的输出：

```
[[2. 1. 4. 3.]
 [1. 2. 3. 4.]
 [4. 3. 2. 1.]]
<NDArray 3x4 @cpu(0)>
```

有些情况下，我们需要随机生成`NDArray`中每个元素的值。下面我们创建一个形状为(3, 4)的`NDArray`。它的每个元素都随机采样于均值为0、标准差为1的正态分布。

```python
nd.random.normal(0, 1, shape=(3, 4))
```

你可以看到这样的输出：

```
[[ 2.2122064   0.7740038   1.0434405   1.1839255 ]
 [ 1.8917114  -1.2347414  -1.771029   -0.45138445]
 [ 0.57938355 -1.856082   -1.9768796  -0.20801921]]
<NDArray 3x4 @cpu(0)>
```

---

## 2 基本运算

`NDArray`支持大量的运算符（operator）。例如，我们可以对之前创建的两个形状为(3, 4)的`NDArray`做按元素加法。所得结果形状不变：

```python
X + Y
```

你会得到这样的输出：

```
[[ 2.  2.  6.  6.]
 [ 5.  7.  9. 11.]
 [12. 12. 12. 12.]]
<NDArray 3x4 @cpu(0)>
```

按元素乘法如下：

```python
X * Y
```

你会看到这样的输出：

```
[[ 0.  1.  8.  9.]
 [ 4. 10. 18. 28.]
 [32. 27. 20. 11.]]
<NDArray 3x4 @cpu(0)>
```

按元素除法如下：

```python
X / Y
```

你会看到这样的输出：

```
[[ 0.    1.    0.5   1.  ]
 [ 4.    2.5   2.    1.75]
 [ 2.    3.    5.   11.  ]]
<NDArray 3x4 @cpu(0)>
```

按元素做指数运算如下：

```python
Y.exp()
```

你会得到这样的输出：

```
[[ 7.389056   2.7182817 54.59815   20.085537 ]
 [ 2.7182817  7.389056  20.085537  54.59815  ]
 [54.59815   20.085537   7.389056   2.7182817]]
<NDArray 3x4 @cpu(0)>
```

除了按元素计算外，我们还可以使用`dot`函数做矩阵乘法。下面将`X`与`Y`的转置做矩阵乘法。由于`X`是3行4列的矩阵，`Y`转置为4行3列的矩阵，因此两个矩阵相乘得到3行3列的矩阵。

```python
nd.dot(X, Y.T)
```

你会得到这样的输出：

```
[[ 18.  20.  10.]
 [ 58.  60.  50.]
 [ 98. 100.  90.]]
<NDArray 3x3 @cpu(0)>
```

我们也可以将多个`NDArray`连结（concatenate）。下面分别在行上（维度0，即形状中的最左边元素）和列上（维度1，即形状中左起第二个元素）连结两个矩阵。可以看到，输出的第一个`NDArray`在维度0的长度$(6)$为两个输入矩阵在维度0的长度之和$(3+3)$，而输出的第二个`NDArray`在维度1的长度$(8)$为两个输入矩阵在维度1的长度之和$(4+4)$。

```python
nd.concat(X, Y, dim=0), nd.concat(X, Y, dim=1)
```

你会得到这样的输出：

```
([[ 0.  1.  2.  3.]
  [ 4.  5.  6.  7.]
  [ 8.  9. 10. 11.]
  [ 2.  1.  4.  3.]
  [ 1.  2.  3.  4.]
  [ 4.  3.  2.  1.]]
 <NDArray 6x4 @cpu(0)>,
 [[ 0.  1.  2.  3.  2.  1.  4.  3.]
  [ 4.  5.  6.  7.  1.  2.  3.  4.]
  [ 8.  9. 10. 11.  4.  3.  2.  1.]]
 <NDArray 3x8 @cpu(0)>)
```

使用条件判断式可以得到元素为0或1的新的`NDArray`。以`X == Y`为例，如果`X`和`Y`在相同位置的条件判断为真（值相等），那么新的`NDArray`在相同位置的值为1；反之为0。

```python
X == Y
```

你可以看到这样的输出：

```
[[0. 1. 0. 1.]
 [0. 0. 0. 0.]
 [0. 0. 0. 0.]]
<NDArray 3x4 @cpu(0)>
```

对`NDArray`中的所有元素求和得到只有一个元素的`NDArray`。

```python
X.sum()
```

你可以看到这样的输出：

```
[66.]
<NDArray 1 @cpu(0)>
```

我们可以通过`asscalar`函数将结果变换为Python中的标量。下面例子中`X`的L2L2范数结果同上例一样是单元素`NDArray`，但最后结果变换成了Python中的标量。

```python
X.norm().asscalar()
```

你可以看到这样的输出：

```
22.494442
```

我们也可以把`Y.exp()`、`X.sum()`、`X.norm()`等分别改写为`nd.exp(Y)`、`nd.sum(X)`、`nd.norm(X)`等。

---

## 3 广播机制

前面我们看到如何对两个形状相同的`NDArray`做按元素运算。当对两个形状不同的`NDArray`按元素运算时，可能会触发广播（broadcasting）机制：先适当复制元素使这两个`NDArray`形状相同后再按元素运算。

先定义两个`NDArray`：

```python
A = nd.arange(3).reshape((3, 1))
B = nd.arange(2).reshape((1, 2))
A, B
```

你可以看到这样的输出：

```
([[0.]
  [1.]
  [2.]]
 <NDArray 3x1 @cpu(0)>,
 [[0. 1.]]
 <NDArray 1x2 @cpu(0)>)
```

由于`A`和`B`分别是3行1列和1行2列的矩阵，如果要计算`A + B`，那么`A`中第一列的3个元素被广播（复制）到了第二列，而`B`中第一行的2个元素被广播（复制）到了第二行和第三行。如此，就可以对2个3行2列的矩阵按元素相加。

```python
A + B
```

你可以看到这样的输出：

```
[[0. 1.]
 [1. 2.]
 [2. 3.]]
<NDArray 3x2 @cpu(0)>
```

---

## 4 索引

在`NDArray`中，索引（index）代表了元素的位置。`NDArray`的索引从0开始逐一递增。例如，一个3行2列的矩阵的行索引分别为0、1和2，列索引分别为0和1。

在下面的例子中，我们指定了`NDArray`的行索引截取范围`[1:3]`。依据左闭右开指定范围的惯例，它截取了矩阵`X`中行索引为1和2的两行。

```python
X[1:3]
```

你可以看到这样的输出：

```
[[ 4.  5.  6.  7.]
 [ 8.  9. 10. 11.]]
<NDArray 2x4 @cpu(0)>
```

我们可以指定`NDArray`中需要访问的单个元素的位置，如矩阵中行和列的索引，并为该元素重新赋值。

```python
X[1, 2] = 9
X
```

你可以看到这样的输出：

```
[[ 0.  1.  2.  3.]
 [ 4.  5.  9.  7.]
 [ 8.  9. 10. 11.]]
<NDArray 3x4 @cpu(0)>
```

当然，我们也可以截取一部分元素，并为它们重新赋值。在下面的例子中，我们为行索引为1的每一列元素重新赋值。

```python
X[1:2, :] = 12
X
```

你可以看到这样的输出：

```
[[ 0.  1.  2.  3.]
 [12. 12. 12. 12.]
 [ 8.  9. 10. 11.]]
<NDArray 3x4 @cpu(0)>
```

---

## 5 运算的内存开销

在前面的例子里我们对每个操作新开内存来存储运算结果。举个例子，即使像`Y = X + Y`这样的运算，我们也会新开内存，然后将`Y`指向新内存。为了演示这一点，我们可以使用Python自带的`id`函数：如果两个实例的ID一致，那么它们所对应的内存地址相同；反之则不同。

```python
before = id(Y)
Y = Y + X
id(Y) == before
```

输出是这样的:

```
False
```

如果想指定结果到特定内存，我们可以使用前面介绍的索引来进行替换操作。在下面的例子中，我们先通过`zeros_like`创建和`Y`形状相同且元素为0的`NDArray`，记为`Z`。接下来，我们把`X + Y`的结果通过`[:]`写进`Z`对应的内存中。

```python
Z = Y.zeros_like()
before = id(Z)
Z[:] = X + Y
id(Z) == before
```

输出是这样的：

```
True
```

实际上，上例中我们还是为`X + Y`开了临时内存来存储计算结果，再复制到`Z`对应的内存。如果想避免这个临时内存开销，我们可以使用运算符全名函数中的`out`参数。

```python
nd.elemwise_add(X, Y, out=Z)
id(Z) == before
```

输出是：

```
True
```

如果`X`的值在之后的程序中不会复用，我们也可以用 `X[:] = X + Y` 或者 `X += Y` 来减少运算的内存开销。

```python
before = id(X)
X += Y
id(X) == before
```

输出是这样的：

```
True
```



## NDArray和NumPy相互转换

我们可以通过`array`函数和`asnumpy`函数令数据在`NDArray`和NumPy格式之间相互变换。下面将NumPy实例变换成`NDArray`实例。

```python
import numpy as np

P = np.ones((2, 3))
D = nd.array(P)
D
```

避讳看到这样的输出：

```
[[1. 1. 1.]
 [1. 1. 1.]]
<NDArray 2x3 @cpu(0)>
```

再将`NDArray`实例变换成NumPy实例。

```python
D.asnumpy()
```

你会看到这样的输出：

```
array([[1., 1., 1.],
       [1., 1., 1.]], dtype=float32)
```

在使用一些工具比如`matplotlib`时，可能需要讲运算过程中产生的`NDArray`转换成`NumPy`再传递给工具。

