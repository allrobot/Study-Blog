---
layout: article
title:  "吴恩达机器学习笔记 Day 4"
date:   2022-02-16
categories: 机器学习
tags: [机器学习,吴恩达]
---
<!-- ./images/ 
$\displaystyle\underbrace{a_i}_{\text{i从1到n}}$

$\displaystyle\mathop{a_i}\limits_{i\text{从1到}n}$
-->
## 非线性模型

关于住房的分类，假设房子有很多特征，面积、卧室、门、房龄等等100多个参数，预测函数表现为：

$$
\begin{equation*} 

g(\theta_{0}+\theta_{1}x_{2}+\theta_{1}x_{2}+\theta_{3}x_{1}x_{2}+\theta_{4}x_{1}^{2}x_{2}\ldots\theta_{100}x_{99}x_{100}^{2}\ldots

\end{equation*}
$$

逻辑回归的二阶多项式二次项有多少呢，通过$\frac{n^2}{2}$可知参数大约5000个，三阶多项式约17000个三次项，特征数太大，会使特征空间急剧膨胀，这并非非线性模型的最佳解决方案。

图像识别中，提取车辆把手的像素点亮度矩阵：

![](./images/2022-02-16/1.png)

![](./images/2022-02-16/2.png)

车辆图片只有50x50像素点，那么n为2500个特征，每个特征值范围为0~255灰度值，假如图片为彩色RGB，RGB包含红蓝绿三个值，特征共n=7500个。

灰色图片的2500个特征用来建立非线性模型，二阶多项式的参数$\frac{n^2}{2}$约等于300万特征，计算量庞大，不利于计算。

## 神经网络

![](./images/2022-02-16/3.png)

$a_{i}^{(j)}$=第j层第i个的激活输出

$\theta^{(j)}$=第j层映射到j+1层的参数矩阵

第j层神经网络有$s_{j}$个神经单元，第$j+1$层有$s_{j+1}$神经单元，此时$\theta^{(j)}$参数的维度为$s_{j+1}*(s_{j}+1)$

第一层的神经网络有3个神经单元，第二层有3个神经单元，第一层与第二层的参数$\theta$为3*(3+1)=12个

$$
\begin{equation*} 

a_{1}^{(2)}=g(\theta_{10}^{(1)}x_{0}+\theta_{11}^{(1)}x_{1}+\theta_{12}^{(1)}x_{2}+\theta_{13}^{(1)}x_{3})  \\
a_{1}^{(2)}=g(\theta_{20}^{(1)}x_{0}+\theta_{21}^{(1)}x_{1}+\theta_{22}^{(1)}x_{2}+\theta_{23}^{(1)}x_{3})  \\
a_{1}^{(2)}=g(\theta_{30}^{(1)}x_{0}+\theta_{31}^{(1)}x_{1}+\theta_{32}^{(1)}x_{2}+\theta_{33}^{(1)}x_{3})  \\
h_{\theta}(x)=a_{1}^(3)=g(\theta_{10}^{(2)}a_{0}+\theta_{11}^{(2)}a_{1}+\theta_{12}^{(2)}a_{2}+\theta_{13}^{(2)}a_{3})

\end{equation*}
$$

### 一对多

![四层神经网络](./images/2022-02-16/4.png)

希望模型y能输出步人时[1 0 0 0]，输出汽车时[0 1 0 0]，输出摩托车时[0 0 1 0]等等。

### 损失函数

![](./images/2022-02-16/5.png)

L=神经网络的层数

$s_{l}$=第l层的神经网络神经单元数量

二分类，通常y输出0或1来表示，输出层的神经单元只有1个，$h_{\theta}(x)\epsilon R$,$s_{l}$=1，K=1。

多分类，四层神经网络的$h_{\theta}(x)=R^{k}$，$s_{l}=K\ (k\ge 3)$，神经单元至少3个以上。

逻辑回归的损失函数：

$$\begin{equation*}
  J(\theta)=-\frac{1}{m}[\displaystyle\sum\limits_{i=1}^{m}y^{(i)}log h_{\theta}(x^{(i)})+(1-y^{(i)})log(1-h_{\theta}(x))]+\frac{\lambda}{2m}\sum\limits_{j=1}^{n}\theta_{j}^{2}  \\
\end{equation*}$$

神经网络的损失函数：

$$\begin{equation*}
  h_{\theta}(x)=R^{k}\ (h_{\theta}(x))_{i}=i^{th}output  \\
  J(\theta)=-\frac{1}{m}[\sum\limits_{i=1}^{m}\sum\limits_{k=1}^{K}y_{k}^{(i)}log (h_{\theta}(x^{(i)}))_{k}+(1-y_{k}^{(i)})log(1-(h_{\theta}(x))_{k})]+\frac{\lambda}{2m}\sum\limits_{l=1}^{L-1}\sum\limits_{i=1}^{s_l}\sum\limits_{j=1}^{s_{l}+1}(\theta_{ji}^{(l)})^{2}  \\
\end{equation*}$$

m个训练样本，K个输出层的神经单元，L-1层，第i层的神经单元个数，第j层的神经单元总数+1(含偏置项)

## 反向传播算法

### 梯度下降算法

要求$\mathop{min}\limits_{\theta}J(\theta)$，需要两个条件：<br>
	- $J(\theta)$<br>
	- $\frac{\partial}{\partial\theta_{ij}^{(l)}}J(\theta)$

先计算从第一层的参数到第四层的值，计算过程具体参见<https://www.zybuluo.com/hanbingtao/note/476663>

第四层的输出层的误差项：$\delta_{j}^{(4)}=a_{j}^{(4)}-y_{j}$

第三层的隐藏层的误差项计算：$(\delta^{(3)}=(\theta^(3))^{T}\delta^{(4)}$.\*$g'(z^{(3)})=a^{(3)}$.\*$(1-a^{(3)})$

第二层的隐藏层的误差项计算：$(\delta^{(2)}=(\theta^(2))^{T}\delta^{(3)}$.\*g'$(z^{(2)})=a^{(2)}.*(1-a^{(2)})$

![](./images/2022-02-16/6.png)

$\frac{\partial}{\partial\theta_{ij}^{(l)}}J(\theta)$的偏导数为$a_{j}^{(l)}\delta{i}^{(l+1)}$

### 反向传播过程

训练集${(x^{(1)},y^{(1)}),\ldots,(x^{(m)},y^{(m)})}$

设$\Delta_{ij}^{(l)}=0\ (\text{for all}\ l,i,j)$，其中的$\Delta$是$\delta$的大写，它设一个全零的误差项矩阵，$\delta$作为累加项在迭代中增加<br>
$\text{For}\ i=1\ to\ m$:<br>
$\qquad$设$a^{(1)}=x^{(i)}$，把第i个训练样本的值输入到第一层；<br>
$\qquad$向前传播计算$a^{(l)}\ \text{for}\ l=2,3,\ldots,L$，从输入层前向计算到输出层；<br>
$\qquad$使用$y^{(i)}$计算$\delta^{(L)}=a^{(L)}-y^{(i)}$，计算输出层的误差项；<br>
$\qquad$计算$\delta^{(L-1)},\delta^(L-2),\ldots,\delta^{(2)}$，逐个从L-1层到第二层的误差项传播计算 (不考虑输入层的误差项)；<br>
$\qquad$$\Delta_{ij}^{(l)}:=\Delta_{ij}^{(l)}+a_{j}^{(l)}\delta_{i}^{(l+1)}$ 累加误差项。<br>

$\delta_{ij}$看作一个位于误差项矩阵的坐标，如果$\delta(L)$是矩阵，可以写成：

$$\begin{equation*}
\Delta^{l}:=\Delta^{l}+\delta^{l+1}(a^{(l)})^{T}
\end{equation*}$$

$$\begin{equation*}
  D_{ij}^{(l)}:=\frac{1}{m}\Delta_{ij}^{(l)}+\lambda\theta_{ij}^{(l)}\ \text{if}\ j\ \ne\ 0  \\
  D_{ij}^{(l)}:=\frac{1}{m}\Delta_{ij}^{(l)}\qquad\qquad \text{if}\ j\ =\ 0  \\
  \frac{\partial}{\partial\theta_{ij}^{(l)}}J(\theta)=D_{ij}^{(l)}
\end{equation*}$$



### 损失函数

$$\begin{equation*}
  J(\theta)=-\frac{1}{m}[\sum\limits_{i=1}^{m}\sum\limits_{k=1}^{K}y_{k}^{(i)}log (h_{\theta}(x^{(i)}))_{k}+(1-y_{k}^{(i)})log(1-(h_{\theta}(x))_{k})]+\frac{\lambda}{2m}\sum\limits_{l=1}^{L-1}\sum\limits_{i=1}^{s_l}\sum\limits_{j=1}^{s_{l}+1}(\theta_{ji}^{(l)})^{2} 
\end{equation*}$$

这种适用于一个输出单元(K=1)的情况，多个输出就要设$\lambda$为0

$$\begin{equation*}
  J(\theta)=-\frac{1}{m}[\sum\limits_{i=1}^{m}\sum\limits_{k=1}^{K}y_{k}^{(i)}log (h_{\theta}(x^{(i)}))_{k}+(1-y_{k}^{(i)})log(1-(h_{\theta}(x))_{k})]  \\
\end{equation*}$$

损失函数$cost(i)=y^{(i)}log (h_{\theta}(x^{(i)}))+(1-y^{(i)})log(1-(h_{\theta}(x)))$，可以认为近似于$cost(i)\approx(h_{\theta}(x^{(i)}-y^{(i)})^2$

$\delta_{j}^{(l)}=$第l层的每个$a_{j}^{(l)}$的损失函数值

形式上，$\delta_{j}^{(l)}=\frac{\partial}{\partial z_{j}^{(l)}}cost(i)\qquad (j\ge 0)$，即$cost(i)=y^{(i)}log (h_{\theta}(x^{(i)}))+(1-y^{(i)})log(1-(h_{\theta}(x)))$

## 梯度检验

![梯度数值的估计](./images/2022-02-16/7.png)

梯度下降算法计算蓝色切线的斜率，梯度检验求的是由$\theta+\varepsilon$到$\theta-\varepsilon$的红线的斜率，高$J(\theta+\varepsilon)-J(\theta-\varepsilon)$，宽$J(\theta+2\varepsilon)$，由三角形边长原理求得该点导数的近似值：

$$\begin{equation*}
  \frac{\partial}{\partial\theta^{(l)}}J(\theta)\approx\frac{J(\theta+\varepsilon)-J(\theta-\varepsilon)}{2\varepsilon}
\end{equation*}$$

这里的$\varepsilon$值最好约等于$10^{-4}$左右的值.

特征向量$\theta$

$$\begin{equation*}
\theta\epsilon R^{n}  \\
\theta=\theta_{1},\theta_{2},\theta_{3},\ldots,\theta_{n}  \\
\frac{\partial}{\partial\theta_{1}^{(l)}}J(\theta)\approx\frac{J(\theta_{1}+\epsilon,\theta_{2},\theta_{3},\ldots,\theta_{n})-J(\theta_{1}-\epsilon,\theta_{2},\theta_{3},\ldots,\theta_{n})}{2\epsilon}  \\
\frac{\partial}{\partial\theta_{2}^{(l)}}J(\theta)\approx\frac{J(\theta_{1},\theta_{2}+\epsilon,\theta_{3},\ldots,\theta_{n})-J(\theta_{1}-\epsilon,\theta_{2},\theta_{3},\ldots,\theta_{n})}{2\epsilon}  \\
\ldots  \\
\ldots  \\
\ldots  \\
\frac{\partial}{\partial\theta_{3}^{(l)}}J(\theta)\approx\frac{J(\theta_{1},\theta_{2},\theta_{3},\ldots,\theta_{n}+\epsilon)-J(\theta_{1}-\epsilon,\theta_{2},\theta_{3},\ldots,\theta_{n})}{2\epsilon}

\end{equation*}$$

然后检查得到的近似值与反向传播的梯度，如果仅有后尾数字稍有不同，说明反向传播算法是没问题的，或高级优化算法的导数是正确的，都能很好优化$J(\theta)$。

实现注意：
- 实现反向传播算法，需计算梯度(展开$D^{(1)},D^{(2)},D^{(3)}$)
- 实现梯度检查，需计算所有参数的近似值
- 确保给出的值极为相似
- 关闭梯度检查，开始学习

注意：在训练分类器之前，要禁用梯度检查代码，否则每次梯度下降迭代（或损失函数的内部循环中）计算梯度变得异常缓慢。

## 训练神经网络

    1. 随机初始化权值
	2. 执行前向传播计算以便获取$h_{\theta}$和所有的x^{(i)}值
	3. 执行代码计算损失函数$J(\theta)$
	4. 执行反向传播算法来计算\frac{\partial}{\partial\theta_{jk}^{(l)}}J(\theta)$偏导数
	
	  for i=1:m{
	      使用训练集进行前向和反向计算
		  获取激活函数$a^{(l)}$和$\delta^{(l)}(l=2,\ldots,L)
		  $\Delta^{l}:=\Delta^{l}+\delta^{l+1}(a^{(l)})^{T}$
		  \ldots
	  }
	  \ldots
	  计算偏导数$\frac{\partial}{\partial\theta_{jk}^{(l)}}J(\theta)$
	5.  使用梯度检验来比较$\frac{\partial}{\partial\theta_{jk}^{(l)}}J(\theta)$，反向传播计算得到的偏导数值和近似值估计互相对比，然后禁用梯度检验代码
	6.使用梯度下降或反向传播的高级优化方法，使最小化$J(\theta)$的参数$\theta$






























































