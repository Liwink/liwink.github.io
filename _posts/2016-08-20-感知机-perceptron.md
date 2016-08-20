[TOC]

#### 背景

> The **perceptron** is an *algorithm* for *supervised* learning of *binary classifiers*: functions that can decide whether an input (represented by a vector of numbers) belongs to one class or another.

**感知器**（Perceptron）是 Frank Rosenblatt 在 1957 年发明的一种**人工神经网络**。它可以被视为一种最简单形式的**前馈神经网络**，是一种二元**线性分类器**。

- 前馈神经网络：参数从输入层向输出层单向传播，和递归神经网络不同，它内部不会构成有向环
- 线性分类器：分类的目标是指将具有相似特征的对象聚集。而**线性分类器**则透过特征的线性组合来做出分类的决定。

> 感知器是生物神经细胞的简单抽象。单个神经细胞可被视为一种只有两种状态的机器 —— 激动时为「是」，未激动时为「否」。神经细胞的状态取决于从其它的神经细胞收到的输入信号量，以及突触的强度。当信号量总和超过了某个阈值时，细胞体就会激动，产生电脉冲。电脉冲沿着轴突并通过突触传递到其它神经元。
>
> 为模拟神经细胞行为，与之对应的感知机基础概念被提出，如权量（突触）、偏置（阈值）及激活函数（细胞体）。

#### 定义

> the perceptron is an algorithm for learning a binary classifier: a function that maps its input `x` (a real-valued vector) to an output value `f(x)` (a single binary value):
> $$
> f(x)  =
>   \begin{cases}
>     1       & \quad \text{if } w \cdot x + b > 0 \\
>     0       & \quad \text{otherwise} \\
>   \end{cases}
> $$
>

#### 步骤

找到 *线性可分数据集* 的分界超平面

##### 参数定义

- 输入向量 **z** 经过感知机的输出：
  $$
  y = f(\mathbf{z})
  $$

- 训练数据集:
  $$
  D = \{(\mathbf{x_1}, \mathbf{d_1}), \dots, (\mathbf{x_n}, \mathbf{d_n})\} \\
  x_{n,i} \text{ 第 } n \text{个输入向量的第 } i \text{ 个元素}
  $$

- 时相关（time-dependence）的权值 w:
  $$
  w_i(t) \text{ 是第 } t \text{ 次训练后 } i \text{ 的权值 }
  $$





##### 训练

1. 初始化权值和偏置。权值可以随机初始化为较小值；

2. 对训练数据集中每个点 `j` 进行下面的迭代：

   1. 计算当前（t 时）感知机下计算的输出值：
      $$
      y_j(t) = f[\mathbf{w}(t) \cdot \mathbf{x_j}]
      $$

   2. 更新权值：
      $$
      w_i(t+1) = w_i(t) + (d_j - y_j(t))x_{j,i} \quad \text{for all features } 0 \leq i \leq n.
      $$

   3. 重复上步，直到迭代误差小于用户定义的误差阈值：
      $$
      \frac{1}{s}\sum_{j=1}^{s} |d_j - y_j(t)| < \gamma
      $$





##### 收敛性

感知器是线性分类器，所以如果训练集 **`D` ** 不是线性可分（[linearly separable](https://en.wikipedia.org/wiki/Linear_separability)）就不会得到理想结果。

如果训练集是线性可分的，感知器即可收敛，并且可以得出权值训练次数的上限。

#### 推导：损失函数最小化

训练中 2.2 步是利用 **梯度下降法** 对 **损失函数** 极小化得到的。

- **损失函数** 的最小化对应「统计学习方法」中的学习策略
- **梯度下降法** 对应着学习的算法

损失函数用输入空间中点 x_0 到超平面 S 的距离表示：
$$
\frac{1}{||w||}|\mathbf{w} \cdot \mathbf{x_0} + \mathbf{b}|
$$
不考虑范数 ||w||，则可定义损失函数为：
$$
L(w, b) = - \sum_{x_i \in M} \ y_i(\mathbf{w} \cdot \mathbf{x_0} + \mathbf{b})
$$
采用 **随机梯度下降法**，极小化误差函数，对 w b 求导后对应梯度如下：
$$
\triangledown_wL(w, b) = - \sum_{x_i \in M}y_ix_i \\
\triangledown_bL(w, b) = - \sum_{x_i \in M}y_i \\
$$
在迭代中对 w, b 进行更新：
$$
w \gets w + \eta y_i x_i \\
b \gets b + \eta y_i \\ 
 \quad \text{learning rate }\eta (0 < \eta \leq 1)
$$

#### Python 实现

{% highlight python %}
import numpy as np

class Perceptron:
​    
    def __init__(self, eta=0.01, epochs=50):
        self.eta = eta
        self.epochs = epochs
        
    def train(self, X, y):
    
        # X is numpy.ndarray
        self.w_ = np.zeros(1 + X.shape[1])
        self.errors_ = []
        
        for _ in range(self.epochs):
            errors = 0
            # update w, b if the result is not as expect
            for xi, target in zip(X, y):
                update = self.eta * (target - self._predict(xi))
                self.w_[1:] += update * xi
                self.w_[0] += update
                errors += int(update != 0.0)
            self.erros_.append(errors)
        return self
    
    def _net_input(self, X):
        return np.dot(X, self.w_[1:]) + self.w_[0]
    
    def _predict(self, X):
        return np.where(self._net_input(X) >= 0.0, 1, -1)
{% endhighlight %}







