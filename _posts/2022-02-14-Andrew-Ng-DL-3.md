---
layout: article
title:  "吴恩达机器学习笔记 Day 3"
date:   2022-02-14
categories: 机器学习
tags: [机器学习,吴恩达]
---
<!-- ./images/ 
$\displaystyle\underbrace{a_i}_{\text{i从1到n}}$

$\displaystyle\mathop{a_i}\limits_{i\text{从1到}n}$
-->
## 分类

先从二分类开始，假设$y\epsilon$\{0,1\}，0和1分别输出no、yes：

![](./images/2022-02-12/7.png)

阈值可以设为0.5，当激活函数的加权输入$\ge$0.5则输出1，小于0.5则输出0，如图所示：

>这种叫**Logistic回归（逻辑回归）**。

但增加额外样本后，蓝线拟合情况就不那么好了，离红叉号的偏差较高，如图所示：

![](./images/2022-02-12/8.png)

不推荐线性回归模型用于分类问题。

## 逻辑回归

$$\begin{equation*}
  h_{\theta}(x)=g(\theta^{T}x)
\end{equation*}$$

其中sigmoid激活函数

$$\begin{equation*}
g(z)=\frac{1}{1+e^{-z}}
\end{equation*}$$

>Sigmoid和Logistic函数是同义词

激活函数的z为$\theta^{T}x$

![](./images/2022-02-12/9.png)

z横坐标，$g(z)$纵坐标，左端趋于0，右端趋于1。

假设一个患者的肿瘤，模型输出肿瘤大小x对于患者是有害还是无害的：

$$\begin{equation*}
x=\begin{bmatrix}
x_{0}  \\
x_{1}
\end{bmatrix}
=\begin{bmatrix}
1  \\
\text{肿瘤面积参数1}  \\
\end{bmatrix}
h_{\theta}(x)=0.7
\end{equation*}$$

说明患者的肿瘤输出y=1的概率为0.7，写成数学表达式：$P(y=1\|x;\theta)$，$\theta$为概率。
y=1的表达式：$P(y=1\|x;\theta)=1-P(y=1\|x;\theta)$

### 决策边界

决策边界(decision boundary)，下图的紫线是决策边界：

![](./images/2022-02-12/10.png)

$$\begin{equation*}
  h_{\theta}(x)=g(\theta_{0}+\theta_{1}x_{1}+\theta_{2}x_{2})  \\
\end{equation*}$$

输出

$$\begin{equation*}
  \theta^{T}x=-3+x_{1}+x_{2}\ge 0  \\
\end{equation*}$$

所以

$$\begin{equation*}
  x_{1}+x_{2}\ge 3  \\
  x_{1}+x_{2}< 3  \\

\end{equation*}$$

![](./images/2022-02-12/11.png)

$$\begin{equation*}
  h_{\theta}(x)=g(\theta_{0}+\theta_{1}x_{1}+\theta_{2}x_{2}+\theta_{3}x_{3}+\theta_{4}x_{4})  \\
\end{equation*}$$

$\theta$初始化为{-1，0，0，1，1}：

$$\begin{equation*}
  \theta=
  \begin{bmatrix}
  -1  \\ 0  \\ 0  \\ 1  \\ 1  \\
  \end{bmatrix}
\end{equation*}$$

如果y=1

$$\begin{equation*}
  -1+x_{1}^{2}+x_{2}^{2}\ge 0  \\
\end{equation*}$$

那么

$$\begin{equation*}
  x_{1}^{2}+x_{2}^{2}\ge 1
\end{equation*}$$

决策边界是取决于参数的，并非通过训练训练集得出的结果，参数不同会得到不同的决策边界。

复杂点的例子：

$$\begin{equation*}
  h_{\theta}(x)=g(\theta_{0}+\theta_{1}x_{1}+\theta_{2}x_{2}+\theta_{3}x_{3}+\theta_{4}x_{1}^{2}x_{2}+\theta_{4}x_{1}^{2}x_{2}^{2}+\theta_{4}x_{1}^{3}x_{2}+\ldots)  \\

\end{equation*}$$

![](./images/2022-02-12/12.png)
![](./images/2022-02-12/13.png)

### 损失函数

给出训练集$(x^{(i)},y^{(i)})$，i维特征向量矩阵，以及激活函数：

$(x^{(1)},y^{(1)}),x^{(2)},y^{(2)}),\ldots,(x^{(n)},y^{(n)})$

$$\begin{equation*}
  x^{(i)}=
  \begin{bmatrix}
    x_{0}^{(i)}  \\
    x_{1}^{(i)}  \\
    \ldots     \\
    x_{n}^{(i)}  
  \end{bmatrix}
  x_{0}=1,\ 0\ <\ y\ <\ 1  \\
  h_{\theta}(x)=\frac{1}{1+e^{-z}}
\end{equation*}$$

接下来$\theta$目标值如何优化？

原来的线性回归模型的损失函数，把$\frac{1}{2}$移至右边函数里面：

$$\begin{equation*}
  J(\theta_{0},\theta_{1})=\frac{1}{2m}\displaystyle\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})^2  \\
  
  Cost(h_{\theta}(x^{(i)}),y^{(i)})=\frac{1}{2}(h_{\theta}(x^{(i)}),y^{(i)})^2
\end{equation*}$$

这种损失函数在线性回归模型比较能良好工作，线性回归的非凹函数和凹函数，通常是希望凹函数能很好收敛全局最优解。

![](./images/2022-02-12/14.png)

代价函数可能实际是非凹函数图的，迭代有时经常收敛到局部最优解，为了达到理想中的梯度下降，需要推导新的梯度下降算法来收敛全局最小值的。

$$\begin{equation*}
  Cost(h_{\theta}(x),y)=
  \begin{cases}-log(h_{\theta}(x)) & \text{if}\ y=1  \\
  -log(1-h_{\theta}(x)) & \text{if}\ y=0  
  \end{cases}
\end{equation*}$$

当y=1时，纵坐标为$J(\theta)$

![](./images/2022-02-12/15.png)

y值无限趋于1，当$y=1,h_{\theta}(x)=1$时，损失函数输出为0，如果$h_{\theta}(x)$趋于0，那么损失趋于无穷。

如果$h_{\theta}=0$，$P(y=1\|x;\theta)=0$，y=1的概率趋于0。假如y=1的话，那么损失函数的值就很大了，用很大的损失来训练这个学习算法，使之下次确保输出1的概率趋于1.

当y=0时，原理是一样的：

$h_{\theta}=1$，$P(y=0\|x;\theta)=0$

![](./images/2022-02-12/16.png)

### 简化损失函数

$$\begin{equation*}
  J(\theta)=\frac{1}{m}\displaystyle\sum\limits_{i=1}^{m}Cost(h_{\theta}(x^{(i)},y^{(i)})  \\
  Cost(h_{\theta}(x),y)=
  \begin{cases}-log(h_{\theta}(x)) & if\ y=1  \\
  -log(1-h_{\theta}(x)) & if\ y=0  
  \end{cases}
\end{equation*}$$

为了避免梯度下降算法按两种情况写，把两个式子合并:

$$\begin{equation*}
  Cost(h_{\theta}(x),y)=-ylog(h_{\theta}(x))-(1-y)log(1-h_{\theta}(x))  \\
\end{equation*}$$

逻辑回归的损失函数，可以这么写的：

$$\begin{equation*}
  
  \begin{split}
  J(\theta)
  & =\frac{1}{m}Cost(h_{\theta}(x^{(i)},y^{(i)})  \\
  & =-\frac{1}{m}[\displaystyle\sum\limits_{i=1}^{m}y^{(i)}log h_{\theta}(x^{(i)})+(1-y^{(i)})log(1-h_{\theta}(x))]  \\
  \end{split}
\end{equation*}$$

公式是由统计学的极大似然法决定的，统计学为不同模型快速找参数，给出此方法(原理和证明略)，它的优点是凸函数。

### 梯度下降算法

$$\begin{equation*} min_{\theta}J(\theta):  \\
\theta_{j}:=\theta_{j}-\alpha \frac{\partial}{\partial\theta_{j}}J(\theta)
\end{equation*}$$

在此式中，$\frac{\partial}{\partial\theta_{j}}J(\theta)$多元微分推导成：

$$\begin{equation*} \frac{\partial}{\partial\theta_{j}}J(\theta)=\frac{1}{m}\displaystyle\sum\limits_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}
\end{equation*}$$

所以导数项：

$$\begin{equation*} \theta_{j}:=\theta_{j}-\alpha \displaystyle\sum\limits_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}
\end{equation*}$$

有若干$\theta$参数，更新参数值需要用到这个公式。

线性回归和逻辑回归有什么区别？它们的梯度下降算法不是同一个，拟合数据也比线性回归更佳，目前已广泛应用于机器学习领域中。

## 高级优化算法

优化算法，给一个$\theta$，需要代码去计算损失函数和偏导数：

- $J(\theta)$
- $\frac{\partial}{\partial\theta_{j}}J(\theta) \qquad (for\ j=0,1,\ldots,n)$

然后利用梯度下降算法去优化目标值：

$$\begin{equation*} min_{\theta}J(\theta):  \\
\theta_{j}:=\theta_{j}-\alpha \frac{\partial}{\partial\theta_{j}}J(\theta)
\end{equation*}$$

除了常规优化算法，还有一些高级优化算法：

- 批量梯度下降算法
- 共轭梯度法
- BFGS
- L-BFGS

已知的梯度下降算法就不介绍了，简单介绍其它三种优化算法的优点：
优点：
	- 不需要手动选择学习率
	- 比常规梯度下降算法快多了
	
缺点：
	- 非常复杂
	- 漫长的学习时长

这些算法给出计算导数项和损失函数的方法，它们有线性搜索算法(智能内循环clever inner-loop)，每次迭代自动尝试不同学习率，所以收敛速度远远比常规梯度下降算法快多了。

细节待补充...可能要学几周时间

## 多分类：一对多

二分类  VS  多分类：

![](./images/2022-02-12/17.png)

三种符号来代表三个不同类别的样本，逻辑回归模型能解决二类别问题，将**训练集分为正类和负类**，这种思想可以用于多分类：

![](./images/2022-02-12/18.png)

把三角形划分正类，其它作为圆形划分到负类，那么就可以训练一个标准的**逻辑回归分类器**，类推红叉号、蓝正方形**可以这么划分**。

拟合出3个分类器，根据$h_{\theta}^{(i)}=P(y=i\|x;\theta)=0\qquad(i=1,2,3)$来估计出x的概率，$\displaystyle\mathop{max}\limits_{i}h_{\theta_{\theta}^{(i)}(x)}$**哪个概率大,分别对应各自的分类**。

每个分类器针对一种情况进行训练

## 过度拟合的问题

![](./images/2022-02-12/19.png)

第一个图，函数模型的直线不能很好拟合训练集，房价上升处是比较平缓、曲线的，这叫**欠拟合**，这算法具有**高偏差**，因为它和训练集理想的情况有很大的偏差；

第二个图，加入二项式，它能很好**拟合**训练集；

第三个图，四项式模拟的曲线上下波动，看似很好的拟合训练集(正好通过每一个训练样本），实际并不是预测房价的好模型，这种称为**过度拟合**，算法具有**高方差**。

高阶多项式虽然能拟合训练集，但它面临庞大的变量问题，导致训练进程缓慢，再千方百计拟合训练集，只要不能泛化到新的样本就是白搭。

>泛化：模型应用到新样本的能力

![](./images/2022-02-12/20.png)

![](./images/2022-02-12/21.png)

解决方案：
	1. 减少特征变量$\theta$
		- 手动保留特征变量
		- 模型选择算法
	2. 正则化
		- 保留所有特征，减小参数的量级或参数$\theta_{j}$的大小
		- 训练更多的训练样本，它能很好工作

## 损失函数正则化

![](./images/2022-02-12/22.png)

高阶多项式，由于过拟合现象导致预测效果不太理想，为了解决应添加正则化参数，$\lambda$参数设为1000，因为数值较大，$\theta_{3}$和$\theta_{4}$的**数值尽可能小**。

$$\begin{equation*} 

\mathop{min}\limits{\theta}\frac{1}{2m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})^{2}+1000\ \theta_{3}^3 + 1000\ \theta_{4}^2

\end{equation*}$$

$\theta_{3}\ \approx\ 0\qquad\theta_{3}\ \approx\ 0$，这样取得更好的模型，增大了两个参数带来的效果。在实际测试中，结果表明**参数越接近0**，得到的函数越**平滑、简单**(原理证明略)。

假设预测房价的模型的特征为$x_{1},x_{2},\ldots,x_{100}$，参数为$\theta_{0},\theta_{1},\theta_{2},\ldots,\theta_{100}$

### 正则化算法

$$\begin{equation*} 

J(\theta)=\frac{1}{2m}[\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})^{2}+\lambda\sum\limits_{i=1}^m\theta_{j}^2]

\end{equation*}$$

如果$\lambda$值取得太大，会造成惩罚值变大，导致迭代后的参数几乎接近0，最后结果为欠拟合。

## 线性回归的正则化

### 正则化损失函数:

$$
\begin{equation*} 
  J(\theta)=\frac{1}{2m}[\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}+\lambda\sum\limits_{i=1}^n \theta_{j}^2]
\end{equation*}
$$

### 梯度下降算法：

$\theta_{0}$已经算过了：

$$
\begin{equation*} 
  \theta_{0}:=\theta_{0}-\alpha\qquad\frac{1}{m}\sum\limits_{i=1}^m \mathop{(h_{\theta}(x^{(i)})-y^{(i)})x_{0}^{(i)}}
\end{equation*}
$$

计算正则化损失函数对$\theta_{j}$，即$\frac{\partial}{\partial \theta_{j}}J(\theta)$偏导数

$$
\begin{equation*} 
  \theta_{j}:=\theta_{j}-\alpha[\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}+\frac{\lambda}{m}\theta_{j}]
\end{equation*}
$$

$$
\begin{equation*} 
  \theta_{j}:=\theta_{j}(1-\alpha\frac{1}{m})-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}
\end{equation*}
$$

训练所有训练样本后，加起来再除以m进行迭代$\theta$参数。正则化的线性回归，它不迭代$\theta_{0}$参数，因为正则化是计算$j=1,2,3,\ldots,n$惩罚值的，要分开计算两种参数的惩罚值。



这里的$\theta_{j}(1-\alpha\frac{1}{m})<1$，学习率很小，m是较大的数值，因此$\alpha\frac{1}{m}$是一个正数，假如该项值=0.99，那么$\theta*0.99$值会变得小一点，也就是在$\theta_{j}^2$的平方范数值变小了。

## 正规方程的正则化

设计一个$m*(n+1)$的矩阵X，X的每行代表一个训练样本，y矩阵为训练集的标签：

$$
  \begin{equation*} 
X=
\begin{bmatrix}
(x^{(1)})^{T}  \\
\ldots  \\
(x^{(m)})^{T}  
\end{bmatrix}
\qquad\qquad\qquad
y=
\begin{bmatrix}
y^{(1)}  \\
\ldots   \\
y^{(m)}  
\end{bmatrix}
\end{equation*}
$$

为了最小化损失函数，$\mathop{min}\limits{\theta}J(\theta)$

### 梯度下降算法

$\frac{\partial}{\partial \theta_{j}}J(\theta)$设各个参数为0进行偏导
$$
\begin{equation*} 
\theta=(x^{T}X+\lambda
\begin{bmatrix}
0&0&0&\ldots&0  \\
0&1&0&\ldots&0  \\
0&0&1&\ldots&0  \\
0&0&0&\ldots&0  \\
0&0&0&\ldots&1
\end{bmatrix}
)^{-1}X^{T}y

\end{equation*}
$$

设n=2的话，上面公式的矩阵是3x3，因此矩阵是(n+1)x(n+1)，按公式去计算可以取得$J(\theta)$最小值。

### 正规方程不可逆

当训练样本$m\ \le\$特征$n$时，X转置乘以X的矩阵是不可逆的，算法：

$$
\begin{equation*} 
\theta=(x^{T}X)^{-1}X^{T}y
\end{equation*}
$$

解决方案是减少$\theta_{j}$参数来解决的，正则化考虑到这点，只要正则化参数，严格要求**$\lambda$大于0**，一般可以解决不可逆的问题。

$$
\begin{equation*} 
\theta=(x^{T}X+\lambda
\begin{bmatrix}
0&0&0&\ldots&0  \\
0&1&0&\ldots&0  \\
0&0&1&\ldots&0  \\
0&0&0&\ldots&0  \\
0&0&0&\ldots&1
\end{bmatrix}
)^{-1}X^{T}y

\end{equation*}
$$

## 逻辑回归的正则化

![](./images/2022-02-12/23.png)

逻辑回归的函数有众多相关度较低的特征，损失函数为：

$$
\begin{equation*} 
-\frac{1}{m}[\displaystyle\sum\limits_{i=1}^{m}y^{(i)}log h_{\theta}(x^{(i)})+(1-y^{(i)})log(1-h_{\theta}(x))]

\end{equation*}
$$

正则化的逻辑回归损失函数：

$$
\begin{equation*} 
-\frac{1}{m}[\displaystyle\sum\limits_{i=1}^{m}y^{(i)}log h_{\theta}(x^{(i)})+(1-y^{(i)})log(1-h_{\theta}(x))]+\frac{\lambda}{m}\theta_{j}
\end{equation*}
$$

依旧是针对$\theta_{1},\theta_{2},\ldots,\theta_{n}$参数，关于参数的更新，还是分开对待的。

### 梯度下降算法

$$
\begin{equation*} 
  \theta_{j}:=\theta_{j}(1-\alpha\frac{1}{m})-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}  \\
  \theta_{j}:=\theta_{j}(1-\alpha[\frac{1}{m})-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}+\frac{\lambda}{m}\theta_{j}]  \\
\end{equation*}
$$

其中的$\frac{1}{m})-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x^{(i)})-y^{(i)})x_{j}^{(i)}+\frac{\lambda}{m}\theta_{j}$是损失函数

关于更高级的优化算法使用正则化，假设有n个参数的n维矩阵：

$$
\begin{equation*} 
\theta=
\begin{bmatrix}
\theta_{0}  \\
\theta_{1}  \\
\theta_{2}  \\
\ldots  \\
\theta_{n}
\end{bmatrix}
\end{equation*}
$$

### 优化算法

先计算所有样本的$J(\theta)$：

$$
\begin{equation*} 
-\frac{1}{m}[\displaystyle\sum\limits_{i=1}^{m}y^{(i)}log h_{\theta}(x^{(i)})+(1-y^{(i)})log(1-h_{\theta}(x))]+\frac{\lambda}{m}\theta_{j}
\end{equation*}
$$

然后计算$\frac{\partial}{\partial\theta_{0}}J(\theta)$对\theta_{0}$的偏导数：
$$
\begin{equation*} 
  \frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x_{0}^{(i)})
\end{equation*}
$$
$\frac{\partial}{\partial\theta_{1}}J(\theta)$对于$\theta_{1}$的导数项：
$$
\begin{equation*} 
  [\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x_{1}^{(i)})]+\frac{\lambda}{m}\theta_{1}
\end{equation*}
$$
$\frac{\partial}{\partial\theta_{2}}J(\theta)$对于$\theta_{2}$导数项：
$$
\begin{equation*} 
  [\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta}(x_{2}^{(i)})]+\frac{\lambda}{m}\theta_{2}
\end{equation*}
$$
后面






























