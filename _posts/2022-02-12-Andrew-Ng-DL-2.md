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

### 多种参数的梯度下降法

之前最简函数$h_{\theta}=\theta_{0}+\theta_{1}x$的$\theta_{0}$，给出的梯度下降法：

$$
\begin{equation*}
\theta_{0}:=\theta_{0}-\vartheta\underbrace{\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}}_{\frac{\partial}{\partial \theta_{0}}J(\theta)}  \\

\theta_{1}:=\theta_{1}-\vartheta\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x^{(i)}
\end{equation*}
$$

意味着当有其它参数时，可给出公式：

$$
\begin{equation*}
\theta_{j}:=\theta_{j}-\vartheta\underbrace{\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x^{(i)}}_{\text{同时更新所有\theta_{j}}}  \\

text{类推  \\}
\theta_{0}:=\theta_{0}-\vartheta\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x_{0}^{(i)}  \\

\theta_{1}:=\theta_{1}-\vartheta\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x_{0}^{(i)}  \\

\theta_{2}:=\theta_{2}-\vartheta\frac{1}{m}\sum\limits_{i=1}^m (h_{\theta_{(i)}-y^{(i)})}x_{0}^{(i)}  \\

\ldots

\end{equation*}
$$

以后的多元梯度下降法用到类似的更新规则。

### 梯度下降训练：特征缩放

假定$x_{1}=面积(0~2000/m^2),x_{2}=卧室数量(1~5)$，呈现的等高线图比较瘦长，梯度下降的点会反复来回震荡很长时间才能抵达全局最优解。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/1.png)

解决办法是**特征缩放**把两个x值设为$0\le x_{1}\le 1 \qquad 0\e x_{2}\le 1$，椭圆等高线图呈现比较扁平，偏移没那么严重。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-12/2.png)

一般特征缩放后的值范围为$-1\le x_{i} \le 1$

$0\le x_{1} \le 3 \qquad -2\le x_{2} \le 0.5  \\
-3 to 3 \qquad -\frac{1}{3} to \frac{1}{3}$







