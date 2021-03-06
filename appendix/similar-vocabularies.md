# 常见的名词和大致释义

## 这里已经包含了的名词

协变量 | 协变量偏移 | 输入 | 量纲 | 无量纲化 | 特征图 | 批量标准化 | 标准化 | 归一化 | 中心化 | 标准化、归一化、中心化的区别 | 特征缩放 | 随机梯度下降

查找一个词条的办法是按下键盘上的`Ctrl+S`，然后键入关键字。

---

- 协变量

  在机器学习和深度学习方法中，协变量指的是与[输入]()无关的其他变量。在计算机视觉中，输入一般指的是图像或特征图（[feature map]()），而协变量可以指权重w等。举个例子：
  $$
  降雨总量S(t) = KTt + e
  $$
  其中，t是自变量时间，降雨量（t）是因变量，而温度（T）则是协变量，K 为一个常数。

  通常情况下，在实验的设计中，协变量是一个独立变量（解释变量），不为实验者所操纵，但仍影响实验结果。

---

- 协变量偏移（covariate shift）

---

- 输入（input）

---

- 量纲（因次，fundamental unit）

  物理学中，不同的物理量有着不同的单位，然而这些单位之间都有相互的联系。实际上，恰当地规定一些基本的单位（称为基本单位），可以使任何其他的单位（称为导出单位）都表达为这些单位的乘积，将其统一以便于研究各个物理量之间的关系。如在国际单位制中，功的单位焦耳（$J$），可以表示为“千克平方米每平方秒”（$kg \cdot m^2 / s^2$）。

  然而，仅仅用单位来表示会面临一些问题：

  1. 在不同的单位制下，各个物理量用单位来表示也会不同，以至于起不到预期的“统一各单位”的效果。如英里每小时（$mph$）与米每秒（$m/s$）乍看之下无甚联系，然而它们却都是表示速度的单位。虽然说经过转换可以将各个基本单位也统一，然而这样终究不够直观，需记忆也不甚方便，而且选择哪一个单位作为统一单位似乎都不甚公平。
  2. 把一个既有的单位表达为拆分了的基本单位的形式实际上没有任何意义，功的单位无论如何都不是“千克二次方米每二次方秒”，因为实际上这个单位根本不存在，它只是与“焦耳”恰好相等而已。况且，这样做也会导致一些拆分后相同但实质不同的单位被混淆，如力矩的单位牛米（$N \cdot m$）被拆分后也是（$kg \cdot m^2 /s^2$），然而它与功显然是完全不同的。

  因此量纲被作为表达导出单位组成的专有方式引入物理学中。

---

- 无量纲化

  当在某种运算中，两种不同量纲的变量对运算结果起着同等的作用，或它们对结果的重要性一样，它们往往可以当作同类的量处理，或同时进行[归一化](//todo)处理

---

- 特征图（feature map）

---

- 批量标准化（BN，batch normalization）

  BN是由Google于2015年提出，这是一个深度神经网络训练的技巧，它不仅可以加快了模型的收敛速度，而且更重要的是在一定程度缓解了深层网络中“梯度弥散”的问题，从而使得训练深层网络模型更加容易和稳定。所以目前BN已经成为几乎所有卷积神经网络的标配技巧了。

---

- （样本）归一化（normalization）

  通常来说，样本标准化是指特征工程中的[特征缩放](//todo)过程。

  在数值上，归一化一般将把数据变成$(0,1)$或$(-1,1)$之间的小数。

  归一化/标准化实质是一种线性变换，线性变换有很多良好的性质，这些性质决定了对数据改变后不会造成“失效”，反而能提高数据的表现，这些性质是归一化/标准化的前提。比如有一个很重要的性质：线性变换不会改变原始数据的数值排序。

  在使用梯度下降的方法求解最优化问题时， 归一化/标准化后可以加快梯度下降的求解速度，即提升模型的收敛速度。

  简单的线性数值归一化运算常见的表示形式有：

  1. Min-Max Normalization： $x' = (x - min(x)) / (max(x) - min(x))$
  2. 平均归一化：$x' = (x - μ) / (max(x) - min(x))$

  简单的线性标准化可能存在的缺陷是当有新数据加入时，可能导致$max(x)$和$min(x)$的变化，需要重新定义。

  非线性的归一化可能的表示形式有：

  1. 对数函数转换：$x' = \log(x)$
  2. 反余切函数转换：$x' = \arctan(x) \cdot 2 / π$

  非线性的标准化方法经常用在数据分化比较大的场景，即有些数值很大，有些很小。通过一些数学函数（包括 log、指数，正切等），将原始值进行映射。根据数据分布的情况，可能会决定使用不同的非线性函数的曲线。并没有完全通用的标准化方法。

---

- （样本）标准化，standardization

  在机器学习和深度学习中，我们可能要处理不同种类的资料，例如，音讯和图片上的像素值，这些资料可能是高维度的，资料标准化后会使每个特征中的数值平均变为0(将每个特征的值都减掉原始资料中该特征的平均)、标准差变为1，这个方法被广泛的使用在许多机器学习算法中(例如：支持向量机、逻辑回归和类神经网络)。

---

- 中心化（零均值化，zero centered）

  中心化后要求样本平均值为0，对标准差无要求

---

- 标准化 | 归一化 | 中心化 的区别

  1. 标准化和中心化的区别：标准化是原始分数减去平均数然后除以标准差，中心化是原始分数减去平均数。 所以一般流程为先中心化再标准化。

  2. 归一化和标准化的区别：归一化是将样本的特征值转换到同一量纲下把数据映射到[0,1]或者[-1, 1]区间内，仅由变量的极值决定，因区间放缩法是归一化的一种。标准化是依照特征矩阵的列处理数据，其通过求z-score的方法，转换为标准正态分布，和整体样本分布相关，每个样本点都能对标准化产生影响。它们的相同点在于都能取消由于量纲不同引起的误差；都是一种线性变换，都是对向量X按照比例压缩再进行平移。

---

- 特征缩放（feature scaling）

  特征缩放的必要性：

---

- 随机梯度下降（random / stochastic gradient descent）

