---
layout: article
title:  "吴恩达机器学习笔记 Day 1"
date:   2022-02-11
categories: 机器学习
tags: [机器学习,吴恩达]
---

<!-- https://github.com/allrobot/Study-Blog/raw/main/assets/images/ 
$\displaystyle\underbrace{a_i}_{\text{i从1到n}}$

$\displaystyle\mathop{a_i}\limits_{i\text{从1到}n}$
-->

## 模型描述

线性`回归`，这里的回归是指**分类**问题，用来预测离散值输出。

举个例子：城市不同房子对应不同售价，假设某房子1250英尺，它的售价220000美元，数据集包含卖出的房子面积大小和价格。

$面积/m^{2} (x)$ | $价格/1000美元(y)$
 ---|---
 2014 | 460
 1416 | 212
 1436 | 315
 859 | 178
 ... | ...
 
 **符号**
 
- **m**=训练样本的数量
    - 假如表格有47列，那么m=47
  
- **x's**=输入变量/特征
    - 表格左侧的面积是输入变量或特征
  
- **y's**=输出变量/目标变量
    - 表格右侧的价格

模型的工作是从这个数据中训练的。

$(x,y)$ 

- 表示特定训练样本

$(x^{(i)},y^{(i)})$ 

- 上标i表示第i个训练样本，充当索引作用，$x^{(1)}$是$2104$，$x^{(2)}$是$1416$，$y^{(1)}$是$460$

```mermaid
graph TB;
	A[训练集]
	B[学习算法]
	C[h(函数)]
	D[房子面积]
	E[售价]
	A-->B;
	B-->C;
	D-->C;
	C-->E;
```

得到h函数，房子面积作为x输入，得到y的输出。这种根据房子尺寸进行房价预测的函数，函数取决于于用户用什么来表达。

$$h_{\theta}=\theta_{0}+\theta_{1}x$$

缩写: $$h_{\theta}(x)$$

![线性回归](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/1.PNG)

这种线性回归类型，是取决于x单一变量的。

## 监督学习

![预测房价](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/2.PNG)

y轴是房价，x轴是房子面积，此时用一条直线拟合数据,假设卖家的房子尺寸是$750^2$，那么它的售价为150\*1000美元，如上图所示。

如果函数用二阶多项式来表达，那么拟合数据的表现可能会更佳。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/3.PNG)

这回归问题的价格是一个离散值，预测离散值的输出。

无论采用哪种模型算法，都属于监督学习的类型，监督学习算法目的是给定训练数据集，迭代函数的权重获得正确的答案，下次输入未知的x也能输出需要的答案。

## 无监督学习

假设某函数判断病人的肿瘤是否良性或恶性，用输出0与1来表示，这类有答案的

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/4.PNG)

当这些数据集没有任何标签，没有答案，算法自行学习其中的结构，将其给不同数据自行分类。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/5.PNG)

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/6.PNG)

## 损失函数

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/7.PNG)

$\theta_{0},\theta_{1}$是常数，输出的值可能不同，通过数据集的训练使直线尽可能靠拢、拟合数据

![未分类的数据集](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/7.PNG)

![已分类的数据集](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/8.PNG)

如何得到正确的$\theta_{0},\theta_{1}$呢？

损失函数定义：$\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})^2$，其中$h_{\theta}(x^{(i)})=\theta_{0}+\theta_{1}x^{(i)}$

m为47个训练样本，通过损失函数计算它们的输出值$h_{\theta}$减去实际值$y^{(i)}$的二次方和从而得到的总误差项[^foot1]，目的是尽可能降低这个值。

为了让损失函数变得容易理解，减少至$\frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})^2$

这是线性回归的整体目标函数，目标是减小$\theta_{0},\theta_{1}$的值，使总误差项最小化。

### 损失函数公式

$$\begin{equation}
  J(\theta_{0},\theta_{1})=\frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})^2  \\
  \mathop{minimize}\limits_{\theta_{0},\theta_{1}} \mathop{J(\theta_{0},\theta_{1})}_{\text{损失函数}}
\end{equation}$$

minimize v. 使减少到最低限度

假设的函数模型的参数有各种各样,为了计算方便，这里先假定函数$h_{\theta}=\theta_{1}x^{(i)}$

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/10.PNG)

假定$\theta_{1}=1$

$$\begin{equation*}
  \begin{split}
    J(1)
    & = \frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})^2  \\
	& = \frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (\theta-y^{(i)})^2  \\
	& = \frac{1}{2m}(0^{2}+0^{2}+0^{2})  \\
	& = 0^{2}
  \end{split}
\end{equation*}$$

函数拟合数据得到最好的表现，总误差项为0，这是最理想的情况

假定$\theta_{1}=0.5$

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/11.PNG)

$$\begin{equation*}
  \begin{split}
    J(0.5)
	& = \frac{1}{2m}[(0*5-1)^{2}+(1-2)^{2}+(1*5-3)^{2})] \\
	& = \frac{1}{2*3}(3*5) \\
	& = \frac{3*5}{6} \\
	& = 0.58
  \end{split}
\end{equation*}$$

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/12.PNG)

$$\begin{equation*}
  \begin{split}
    J(0)
	& = \frac{1}{2m}[1^{2}+2^{2}+3^{2})] \\
	& = \frac{1}{6}*14 \\
	& = 2.3
  \end{split}
\end{equation*}$$

同理，当$\theta_{1}=-0.5$时，总误差项为5.25，误差非常大

![梯度](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/13.PNG)

横轴为$\theta_{1}$，纵轴为总误差项，之前$\theta_{1}=1、\theta_{0}=1\ldots$给出的总误差项y，构成上图

损失函数的优化目标是获得最小值的$J(\theta_{1})$，此时上图$\theta_{1}=1$已结完美拟合了

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/14.PNG)

$\theta_{0}=50,\theta_{1}=0.06$,绘制$J(\theta_{0},\theta_{1})$就多了一个$\theta_{0}=50$，它与训练集相关

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/15.PNG)

这是等高线图，用2D表现如下图(几圈圈椭圆)，最小值位于最小的椭圆中心。

首先假定$\theta_{0}=800,\theta_{1}=-0.15$，位于下图$J(\theta_{0},\theta_{1})$手绘篮圈的红叉号。左边的$h(x)$显然不符合，离最小值甚远，因为拟合的不好

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/16.PNG)

假定$\theta_{0}=360,\theta_{1}=0$，离最小值有些接近了

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/17.PNG)
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/18.PNG)

到了这里，虽然不是最小值，这是训练样本实际值与预测值之间的所有误差和，$J(\theta_{0},\theta_{1})$表示损失函数J的意义，接近损失函数J最小值的点。

我们目标是寻找一个高效的算法，来自动寻找损失函数J最小值，之后由于涉及众多参数、更高维的图形，只能通过代码实现。

[^foot1]: 误差项，某些文献也称呼敏感度、错误率

## 梯度下降

用梯度下降算法最小化任意函数J，包括线性函数、非线性函数等。

$J(\theta_{0},\theta_{1})$可扩展$J(\theta_{0},\theta_{1},\theta_{2}\ldots\theta_{n})$

最小化$\mathop{min}\limits_{\theta_{0},\theta_{1}}J(\theta_{0},\theta_{1})$亦可$\mathop{min}\limits_{\theta_{0},\theta_{1}}J(\theta_{0},\theta_{1},\theta_{2}\ldots\theta_{n})$

梯度下降，一般设$\theta_{0}=0,\theta_{1}=0)$，假如能看到哪里是最低点，如下图一步一步走不断收敛至局部最低点。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/19.PNG)

实际上，我们并不能环顾四周看哪里是最低的，是碰运气不断走着，直到获得完全不同的局部最优解[^foot2]。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/20.PNG)

### 梯度下降算法

$$\text{重复直至收敛}{\theta_{j}:=\theta_{j}-\alpha\frac{\partial}{\partial\theta_{j}}J(\theta_{0},\theta_{1})\qquad (\text{同时更新 j=0 和 j=1})}$$

`正确：同时更新`{:.success}
$$temp0:=\theta_{0}-\alpha\frac{\partial}{\partial\theta_{0}}J(\theta_{0},\theta_{1}) \\
temp1:=\theta_{1}-\alpha\frac{\partial}{\partial\theta_{1}}J(\theta_{0},\theta_{1}) \\
\theta_{0}:=temp0  \\
\theta_{1}:=temp1$$

`错误：异步更新`{:.error}
$$temp0:=\theta_{0}-\alpha\frac{\partial}{\partial\theta_{0}}J(\theta_{0},\theta_{1}) \\
\theta_{0}:=temp0  \\
temp1:=\theta_{1}-\alpha\frac{\partial}{\partial\theta_{1}}J(\theta_{0},\theta_{1}) \\
\theta_{1}:=temp1$$

>`:=`{:.info}这是计算机赋值的意思

$\alpha$是**学习率**，如果这个**常数很大**，那么梯度下降$J(\theta_{0},\theta_{1})$容易**越过最小值**，左右横跳导致梯度上升(自己算)；如果**太小**，要迭代**很多次**才能收敛。

反复迭代，直至收敛为止。如果使用异步更新，那就不是梯度下降法了，是其它的算法。

### 梯度下降法的意义

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/21.PNG)

公式的$\frac{\partial}{\partial \theta_{1}}J(\theta_{0})$是取坐标轴曲线一点的切线(红色直线的斜率)，这条线有正斜率，说明它有正导数。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/22.PNG)

因为是正数，$\theta_{1}$减去$\partial$乘以的正数，在坐标轴体现的是向左移动，然后迭代几轮收敛到最小值。

如果$\theta_{1}$在左边，那么$\frac{\partial}{\partial \theta_{1}} J(\theta_{1})$的导数是负的，按公式(3)$\theta_{1}$的值不断递增，直至梯度最小值为止。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/23.PNG)

#### 梯度消失

假设$\theta_{1}$处于局部最优解，这样的话$\frac{\partial}{\partial \theta_{1}}J(\theta_{0},\theta_{1})$的斜率等于0，不上升也不下降。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/24.PNG)

是因为$\frac{\partial}{\partial \theta_{1}}J(\theta_{0},\theta_{1})$的斜率更新，刚开始从右边紫点出发，这时导数值值比较大，绿点随着接近最小值，得到导数值变小了，得到红点，再继续梯度下降，直至收敛到局部极小值。根据定义，在局部最低点时导数等于0，这是梯度下降法的运行方式，所以没必要减小$\partial$的值

$$\begin{equation*}
  \begin{split}
    \frac{\partial}{\partial \theta_{1}}J(\theta_{0},\theta_{1})
	& = \frac{\partial}{\partial \theta_{1}}\frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})^2  \\
	& = \frac{\partial}{\partial \theta_{1}}\frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (\theta_{0}+\theta_{1}x^{(i)}-y^{(i)})^2  \\
	& = 
  \end{split}
\end{equation*}$$

推导函数的两个$\theta$值，按上述推导可以明白(多元微分推导)

对应$\theta_{0}$的$\frac{\vartheta}{\vartheta \theta_{0}}J(\theta_{0},\theta_{1})$的偏导数

$$\begin{equation*}
  \begin{split}
    \theta_{0}
	& = 0:\frac{\partial}{\partial \theta_{0}}J(\theta_{0},\theta_{1})
	& = \frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})
  \end{split}
\end{equation*}$$

对应$\theta_{1}$的$\frac{\vartheta}{\vartheta \theta_{1}}J(\theta_{0},\theta_{1})$的偏导数

$$\begin{equation*}
  \begin{split}
    \theta_{1}
	& = 1:\frac{\partial}{\partial \theta_{1}}J(\theta_{0},\theta_{1})
	& = \frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}-y^{(i)})*x^{(i)}
  \end{split}
\end{equation*}$$

然后$\theta_{0},\theta_{1}$不断重复该过程直到收敛。线性回归问题通常表现为弓状函数(凸函数)，这种看起来像弓形，只有一个全局最优解，当计算这种损失函数的梯度下降，总是收敛到全局最优。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/26.PNG)

房价的数据集，先初始设定$\theta_{0}=-900,\theta_{1}=-0.1$

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/27.PNG)
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/28.PNG)
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/29.PNG)
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/30.PNG)
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/31.PNG)
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-11/32.PNG)

这种梯度下降法叫**"Batch"**梯度下降，遍历所有训练样本并计算偏导数，每一次梯度下降总要计算m个样本的总和。





















[^foot2]: [关于局部最优解]()