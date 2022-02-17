---
layout: article
title:  "吴恩达机器学习笔记 Day 2"
date:   2022-02-12
categories: 机器学习
tags: [机器学习,吴恩达]
---

<!-- https://github.com/allrobot/Study-Blog/raw/main/assets/images/ -->

## 矩阵和向量

矩阵是矩形数组的，由行数i和列数j构成

4\*2矩阵，用数学符号表现为$R^{4*2}$

$$
A=
\begin{equation*}\begin{bmatrix}
1402&191  \\
1372&821  \\
949&1437  \\
147&1448
\end{bmatrix}\end{equation*}
$$

$A_{ij}$，那么$A_{11}=1402,A_{22}=821,A_{32}=1437$

2\*3矩阵，用数学符号表现为$R^{2*3}$

$$
\\
\begin{equation*}\begin{bmatrix}
1&2&3  \\
4&5&6
\end{bmatrix}\end{equation*}
$$

由n\*1的向量矩阵，$R^{4}$
$$
y=
\begin{equation*}\begin{bmatrix}
460  \\
232  \\
315  \\
178  
\end{bmatrix}\end{equation*}
$$

$y_{1}=460,y_{2}=232$

它是从下标1开始的，计算机中是从0开始的。矩阵通常以大写字母表示的，小写字母表现为标量向量的。

### 矩阵计算

$h_{\theta}(x)=-40+0.25x$

那么矩阵计算过程为
$$
\begin{bmatrix}
1&2104  \\
1&1416  \\
1&1534  \\
1&852
\end{bmatrix}
*
\begin{bmatrix}
-40  \\
0.25
\end{bmatrix}
=
\begin{bmatrix}
-40*1+0.25*2104  \\
-40*1+0.25*1416  \\
-40*1+0.25*1534  \\
-40*1+0.25*852
\end{bmatrix}
$$

给定3个假定函数
1. $h_{\theta}(x)=-40+0.25x$
2. $h_{\theta}(x)=200+0.1x$
3. $h_{\theta}(x)=-150+0.4x$

$$
\begin{equation*}
\begin{bmatrix}
1&2104  \\
1&1416  \\
1&1534  \\
1&852
\end{bmatrix}
*
\begin{bmatrix}
-40&200&-150  \\
0.25&0.1&0.4
\end{bmatrix}
=
\begin{bmatrix}
486&410&692  \\
314&342&416  \\
344&353&464  \\
173&285&191
\end{bmatrix}
\end{equation*}
$$

### 矩阵特性

参考《线性代数》矩阵相关的公式、定义等

链接：<https://github.com/allrobot/Study-Blog/raw/main/docs/about_data/线性代数.pdf>

| --- |
| AB未必与BA相等 |
|AX=AY 不能推出X=Y|
|$(AB)^k$与$A^{k}B^{k}$不一定相等|
|$A^{2}+(k+j)AB+kjB^{2}$ 与 $(A+kB)(A+jB)$ 不一定相等,但$A^{2}+(k+j)A+kjE=A^{2}+(k+j)AE+kjE^{2}=(A+kE)(A+jE)$|
| $\|\lambda A\|=\lambda^{n}\|A\|$ |
|$\|(AB)^{T}\|=B^{T}A^{T}$|
|$A\*A^{1}=E\ or\ A^{1}\*A=E$|
|$A\*A^{\*}=\|A\|E\ or\ A^{\*}*A=\|A\|E$|


<table>
	<tr>
		<td>条件</td>
		<td colspan="4">解的情况</td>
	</tr>
	<tr>
		<td rowspan="2">齐次</td>
		<td>$R(A)=$未知数个数</td>
		
		<td colspan="3">唯一解（零解）</td>
		
	</tr>
	<tr>
		<td>$R(A)<$未知数个数</td>
		<td  colspan="3">多个解（零解和多个非零解）</td>
	</tr>
	<tr>
		<td rowspan="3">非齐次</td>
		<td>$R(A)\ne R(A|b)$</td>
		<td colspan="3">无解</td>
	</tr>
	<tr>
		<td rowspan="2">$R(A)=R(A|b)$</td>
		<td rowspan="2">有解</td>
		<td>$R(A)=R(A|b)=$未知数个数</td>
		<td>一个非零解</td>
	</tr>
	<tr>
		<td>$R(A)=R(A|b)<$未知数个数</td>
		<td>多个非零解</td>
	</tr>
</table>
		
## 多种特征

特征向量，以房价为例

面积$m^2$ | 卧室数量 | 门数量 | 房龄 | 价格
---|---|---|---|---
2104 | 5 | 1 | 45 | 460
1416 | 3 | 2 | 40 | 232
1534 | 3 | 2 |30 | 315
852 | 2 | 1 |36 | 178
$\ldots$ | $\ldots$ | $\ldots$ | $\ldots$

- n=特征数量

$$
\begin{equation*}
n=4
\end{equation*}
$$

- $x^{(i)}$=$i^{th}$训练样本的特征向量

$$


\begin{equation*}x^{2}=
\begin{bmatrix}
1416  \\
3  \\
2  \\
40
\end{bmatrix}
\end{equation*}
\epsilon R^{4}

$$

- $x_{j}^{(i)}$=$i^{th}$训练样本的第j个特征量的值

$$
\begin{equation*}
x_{3}^{(2)}=2
\end{equation*}
$$

线性回归的假定函数$h_{\theta}=\theta_{0}+\theta_{1}x$不适用此情况，此时的函数模型应为：

$$
\begin{equation*}
h_{\theta}=\theta_{0}+\theta_{1}x_{1}+\theta_{2}x_{2}+\theta_{3}x_{3}-\theta_{4}x_{4}
\end{equation*}
$$

意味着后续特征增加时，模型可应为:

$$
\begin{equation*}
h_{\theta}=\theta_{0}+\theta_{1}x_{1}+\theta_{1}x_{2}+\ldots+\theta_{n}x_{n}  \\
x=
\begin{bmatrix}
x_{0}  \\
x_{1}  \\
x_{2}  \\
\ldots  \\
x_{n}
\end{bmatrix}\ 
\epsilon\  R^{n+1}
\qquad
\theta=
\begin{bmatrix}
R_{0}  \\
R_{1}  \\
R_{2}  \\
\ldots  \\
R_{n}
\end{bmatrix}
\epsilon\  R^{n+1}  \\
  \begin{split}
    h_{\theta}
    & = \theta_{0}x_{0}+\theta_{1}x_{1}+\theta_{1}x_{2}+\ldots-\theta_{n}x_{n}   \\
	& = \theta_{x}^{T}  \\
  \end{split}  \\
  
\text{其中}  \\

\begin{bmatrix}
\theta_{0}x_{0}&\theta_{1}x_{1}&\ldots&\theta_{n}x_{n}  \\
\end{bmatrix}

\begin{bmatrix}
x_{0}  \\
x_{0}  \\
\ldots  \\
x_{n} 
\end{bmatrix}

\end{equation*}
$$


>为了方便，$\theta_{0}x{0}$的$x{0}=1$,故省去

## 多种参数的梯度下降法

之前最简函数$h_{\theta}=\theta_{0}+\theta_{1}x$的$\theta_{0}$，给出的梯度下降法：

$$
\begin{equation*}
\theta_{0}:=\theta_{0}-\alpha\underbrace{\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}}_{\frac{\partial}{\partial \theta_{0}}J(\theta)}  \\

\theta_{1}:=\theta_{1}-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x^{(i)}
\end{equation*}
$$

意味着当有其它参数时，可给出公式：

$$
\begin{equation*}
\theta_{j}:=\theta_{j}-\alpha\underbrace{\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x^{(i)}}_{\text{同时更新所有\theta_{j}}}  \\

text{类推 }  \\
\theta_{0}:=\theta_{0}-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x_{0}^{(i)}  \\

\theta_{1}:=\theta_{1}-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x_{0}^{(i)}  \\

\theta_{2}:=\theta_{2}-\alpha\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x_{0}^{(i)}  \\

\ldots

\end{equation*}
$$

以后的多元梯度下降法用到类似的更新规则。

## 梯度下降训练：特征缩放

假定$x_{1}=面积(0~2000/m^2),x_{2}=卧室数量(1~5)$，呈现的等高线图比较瘦长，梯度下降的点会反复来回震荡很长时间才能抵达全局最优解。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/1.png)

解决办法是**特征缩放**把两个x值设为$0\le x_{1}\le 1, \  0\le x_{2}\le 1$，椭圆等高线图呈现比较扁平，偏移没那么严重。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/2.png)

一般特征缩放后的值范围为$-1\le x_{i} \le 1$

`正确`{:.success}
$0\le x_{1} \le 3 \qquad -2\le x_{2} \le 0.5  \\ \qquad \qquad
-3\ to\ 3 \qquad -\frac{1}{3}\ to\ \frac{1}{3}$

`错误`{:.error}
$-100\le x_{3}\le 100 \qquad -0.0001\le x_{4}\le 0.0001$

只有这样，梯度下降法才会**正常工作**！

## 均值归一化

用$x_{i}$替换$x_{i}-u_{i}$使特征的均值近似为零(不使$x_{0}=1)

假若房子面积平均值为1000，每套房子的卧室平均数量为2

$$
\begin{equation*}
x_{1}=\frac{\text{面积}-1000}{2000}  \\

x_{2}=\frac{\text{卧室数量}-2}{5}  \\

-0.5\le x_{1}\le 0.5,-0.5\le x_{2}\le 0.5

\end{equation*}
$$

取值范围可以为-1~5一切是为了梯度下降的速度更快。

$$
x_{i}=\frac{x_{1}-u_{1}}{s_{1}}
$$

## 梯度下降训练：学习率

学习率选择比较重要，如果选大了，也许梯度下降只要30步就完成了；选小了，梯度也可能要3000，甚至3，000，000步才能抵达全局最优解。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/3.png)

如何判断梯度下降曲线，从而选择合适的学习率。当判断梯度下降的时候发现$J(\theta)$图正上升，这里学习率可能选大了，应选小，不然会发生梯度爆炸的情况。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/4.png)

也有$J(\theta)$显示波浪线，这里暂不讨论，上述学习率的选择适用于线性回归问题。

有一个解决方案比较适合：

$$\ldots,0.001,\ldots,0.01,\ldots,0.1,\ldots,1,\ldots$$

每迭代10~100轮，学习率0.001递增3倍，若不影响梯度下降就升为0.03，再递增...出现梯度上升的情况就反之。

$$\ldots,0.001,0.003,0.01,\ldots,0.1,\ldots,1,\ldots$$

## 多变量的线性回归
 
![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/5.png)

要设计函数模型输出房子的售价，要考虑提取哪些特征，上图有2个特征，房子正面和纵深的长度乘号得面积，模型也许二次项$h_{\theta}=\theta_{0}+\theta_{1}x+\theta_{2}x^{2}$能很好预测价格，但随着房子面积增加且价格下降，这就不能很好拟合数据集了，改成三次方$h_{\theta}=\theta_{0}+\theta_{1}x+\theta_{2}x^{2}+\theta_{3}x^{3}$

$$\begin{equation*}
  \begin{split}
    h_{\theta}(x)
	& = h_{\theta}=\theta_{0}+\theta_{1}x+\theta_{2}x^{2}+\theta_{3}x^{3} \\
	& = h_{\theta}=\theta_{0}+\theta_{1}\text{(面积)}+\theta_{2}\text{(面积)}^{2}+\theta_{3}\text{(面积)}^{3} \\
  \end{split}
\end{equation*}$$
$$\begin{equation*}
  x_{1}=\text{(面积)}  \qquad \qquad \text{(面积:1~1000)}\\
  x_{2}=\text{(面积)}^2\qquad \qquad \text{(面积:1~10^5)} \\
  x_{3}=\text{(面积)}^3\qquad \qquad \text{(面积:1~10^9)} 
\end{equation*}$$

三个参数的值较大，采用特征缩放为宜。

除此之外，当房子面积增加时二次项函数不太拟合数据集，除了改至三次项，第二个参数可以采用平方根函数：


$$\begin{equation*}
h_{\theta}=\theta_{0}+\theta_{1}\text{(面积)}+\theta_{2}\sqrt{\text{(面积)}}
\end{equation*}$$

特征的选择需要多种合适的角度去看待。

## 梯度下降法：正规方程

正规方程法相比batch梯度下降一次次迭代获取全局最优解，它能**一次性**直接获取$\theta$值。batch梯度下降法，需要分别去计算误差项对各个特征的偏导数$\frac{\partial}{\partial\theta_{j}}J(\theta)$，步骤相当多，推导又麻烦

正规方程法的训练步骤：训练样本m为4个

面积$x_{1}$|卧室数量$x_{2}$|门数量$x_{3}$|房龄$x_{4}$|价格$y$
---|---|---|---|---
2104|5|1|45|460
1416|3|2|40|232
1534|3|2|30|315
852|2|1|36|178

n+1维特征向量
$$\begin{equation*}
  X=
  \begin{bmatrix}
    1&2104&5&1&45&460  \\
    1&1416&3&2&40&232  \\
    1&1534&3&2&30&315  \\
    1&852&2&1&36&178
  \end{bmatrix}
  \qquad
  y=
    \begin{bmatrix}
    460  \\
    232  \\
    315  \\
    178
  \end{bmatrix}
\end{equation*}$$

$m*(n+1)$维矩阵，正规方程法如下

$$\begin{equation*}
  \theta=(X^{T}X)^{-1}X^{T}y
\end{equation*}$$

举个例子，训练样本共m个，它的数据集是:$(x^{(1)},y^{(1)}),x^{(2)},y^{(2)}),\ldots,(x^{(n)},y^{(n)})$，有n个特征。

特征矩阵，有n+1个特征向量
$$\begin{equation*}
  x^{(i)}=
  \begin{bmatrix}
    x_{0}^{(i)}  \\
    x_{1}^{(i)}  \\
    \ldots     \\
    x_{n}^{(i)}
  \end{bmatrix}
\end{equation*}$$


然后计算X的转置
$$\begin{equation*}
  X=
  \begin{bmatrix}
    ——(x^{(1)})^{T}——  \\
    ——(x^{(2)})^{T}——  \\
    \ldots  \\
    ——(x^{(m)})^{T}——
  \end{bmatrix}
\end{equation*}$$

举一个具体的例子：
假如特征矩阵有2个特征向量
$$\begin{equation*}
  x^{(i)}=\begin{bmatrix}
    1  \\
	x_{1}^{(i)}
  \end{bmatrix}
\end{equation*}$$

$$\begin{equation*}
  X=\begin{bmatrix}
    1&x_{1}^{(1)}  \\
	1&x_{2}^{(1)}  \\
	\ldots  \\
	1&x_{m}^{(1)}  \\
	
  \end{bmatrix}
  y=\begin{bmatrix}
    y^{(1)}  \\
	y^{(2)}  \\
	\ldots   \\
	y^{(m)}  \\
  \end{bmatrix}
\end{equation*}$$

这个X矩阵构建后，计算$\theta=(X^{T}X)^{-1}X^{T}y$能一次性训练出$x_{1}$的$\theta$值

**正规方程法**
- 不需要学习率
- 不需要迭代
- 需要计算$(X^{T}X)^{-1}$
- 如果n个特征向量的数很大，计算缓慢

**批次梯度下降法**
- 需要学习率
- 需要迭代
- n个特征向量很大，能很好工作

正规方程法适用计算1万以下的特征向量，1万以上的适用批次梯度下降法。

### 正规方程和不可逆转性

如果出现$X^{T}X$矩阵不可逆，可以考虑特征矩阵删除一些多余的特征，使之可逆。

## 例子代码

### 数据集链接

<https://github.com/Yongsgithub/picturebed/tree/Yongsgithub-NG-Machine-Learning>

```python
import pandas as pd
import seaborn as sns
import numpy as np
from matplotlib import pyplot as plt

df = pd.read_csv('D:\\note\\ex1-linear regression\\ex1data1.txt', names=['横坐标', '纵坐标'])

# # 输出散点图
# sns.lmplot('横坐标', '纵坐标', data=df, size=6, fit_reg=True)
# # fit_reg:拟合回归参数,如果fit_reg=True则散点图中则出现拟合直线
# plt.show()


def computeCost(X, y, theta):
    # 矩阵相乘，正好((x1 * theta_1 + x2 * theta_2)-2)^2
    inner = np.power((X * theta.T) - y, 2)

    # theta.T就是矩阵theta的转置矩阵
    # np.power(A,B)   ## 对A中的每个元素求B次方
    return np.sum(inner) / (2 * len(X))


df.insert(0, 'ONE', 1)

cols = df.shape[1]  # 获取表格df的列数
X = df.iloc[:, 0:cols - 1]  # 除最后一列外，取其他列的所有行，即X为O和人口组成的列表
y = df.iloc[:, cols - 1:cols]  # 取最后一列的所有行，即y为利润

X = np.matrix(X.values)
y = np.matrix(y.values)
theta = np.matrix(np.array([0, 0]))

computeCost(X, y, theta)


def gradientDescent(X, y, theta, alpha, iters):  # alpha是学习率,iters为迭代次数
    temp = np.matrix(np.zeros(theta.shape))  # np.zeros(theta.shape)=[0.,0.],然后将temp变为矩阵[0.,0.]
    parameters = int(theta.ravel().shape[1])
    # theta.ravel()：将多维数组theta降为一维，.shape[1]是统计这个一维数组有多少个元
    # parameters表示参数
    cost = np.zeros(iters)  # 初始化代价函数值为0数组，元素个数为迭代次数

    for i in range(iters):  # 循环iters次
        error = (X * theta.T) - y

        for j in range(parameters):
            term = np.multiply(error, X[:, j])  # 将误差与训练数据相乘，term为偏导数，参考笔记P27
            temp[0, j] = theta[0, j] - ((alpha / len(X)) * np.sum(term))  # 更新theta

        theta = temp
        cost[i] = computeCost(X, y, theta)  # 计算每一次的代价函数

    return theta, cost


alpha = 0.01
iters = 2000

g, cost = gradientDescent(X, y, theta, alpha, iters)  # 令g和cost分别等于函数的两个返回值

x = np.linspace(df.横坐标.min(), df.横坐标.max(), 100)  # 以人口最小值为起点，最大值为终点，创建元素个数为100的等差数列
f = g[0, 0] + (g[0, 1] * x)  # f是假设函数H

# 输出数据集的拟合情况
x = np.linspace(df.横坐标.min(),df.横坐标.max(),100)#以人口最小值为起点，最大值为终点，创建元素个数为100的等差数列
f = g[0,0] + (g[0,1] * x) #f是假设函数H

fig, ax = plt.subplots(figsize=(12,8))#以其他关键字参数**fig_kw来创建图
#figsize=(a,b):figsize 设置图形的大小,b为图形的宽,b为图形的高,单位为英寸
ax.plot(x, f, 'r', label='Prediction')   #设置点的横坐标，纵坐标，用红色线，并且设置Prediction为关键字参数
ax.scatter(df.横坐标, df.纵坐标, label='Traning Data')  #以人口为横坐标，利润为纵坐标并且设置Traning Data为关键字参数
ax.legend(loc=2)  #legend为显示图例函数，loc为设置图例显示的位置，loc=2即在左上方
ax.set_xlabel('Population')  #设置x轴变量
ax.set_ylabel('Profit')  #设置x轴变量
ax.set_title('Predicted Profit vs. Population Size') #设置表头
plt.show()


# 代价数据可视化1
c=pd.DataFrame({'inters':list(range(iters)),'cost':cost  })
ax=sns.lmplot(x='inters',y='cost',data=c,fit_reg=False)
plt.show()


# # 代价数据可视化2
# fig, ax = plt.subplots(figsize=(12,8)) #以其他关键字参数**fig_kw来创建图
# #figsize=(a,b):figsize 设置图形的大小,b为图形的宽,b为图形的高,单位为英寸
# ax.plot(np.arange(iters), cost, 'b') #作图:以迭代次数为x，代价函数值为y,线条颜色为红色
# ax.set_xlabel('Iterations')  #设置x轴变量
# ax.set_ylabel('Cost')  #设置y轴变量
# ax.set_title('Error vs. Training Epoch') #设置表头
# plt.show()

```




