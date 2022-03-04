---
layout: article
title:  "吴恩达机器学习笔记 Day 5"
date:   2022-02-17
categories: 机器学习
tags: [机器学习,吴恩达]
---
<!-- https://github.com/allrobot/Study-Blog/raw/main/assets/images/ 
$\displaystyle\underbrace{a_i}_{\text{i从1到n}}$

$\displaystyle\mathop{a_i}\limits_{i\text{从1到}n}$
-->
## 评估模型

### 验证模型性能

训练集分为三部分：
- 60%训练集traning set
- 20%交叉验证集cross validation set
- 20%测试集test set

60%训练集训练出来的$\theta$参数，最小化$J(\theta)$，然后计算验证集的测试误差$J(\theta)$(线性回归、逻辑回归的$J(\theta)$定义是不同的)。



### 选择多项式模型

1. $h_{\theta}(x)=\theta_{0}+\theta_{1}x$<br>
2. $h_{\theta}(x)=\theta_{0}+\theta_{1}x+\theta_{2}x^2$<br>
3. $h_{\theta}(x)=\theta_{0}+\theta_{1}x+\ldots+\theta_{3}x^3$<br>
   .<br>
   .<br>
   .<br>
10. $h_{\theta}(x)=\theta_{0}+\theta_{1}x+\ldots+\theta_{10}x^{10}$

各个不同多项式模型通过训练集训练的$\theta$参数，然后用验证集验出误差，哪个模型的验证集误差较小就选哪个。

### 判断模型的拟合

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/1.png)

偏差（欠拟合）：
- $J_{训练}(\theta)$值很高，左边的bias
- $J_{交叉验证}(\theta)\approx J_{训练}(\theta)$

方差（过拟合）：
- $J_{训练}(\theta)$值较低，右边的variance
- $J_{交叉验证}(\theta)>> J_{训练}(\theta)$

>\>\>数学符号的意思是远远大于此值

### 正则化和偏差/方差 

模型：
$$
\begin{equation*} 

h_{\theta}(x)=\theta_{0}+\theta_{1}x+\theta_{2}x^{2}+\theta_{3}x^{3}+\theta_{4}x^{4}

\end{equation*}
$$

$$
\begin{equation*} 
J(\theta)=\frac{1}{2m}\sum\limits_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2}+\frac{\lambda}{2m}\sum\limits_{j=1}^{m}\theta_{j}^{2}

\end{equation*}
$$

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/2.png)

$\lambda$值过大，导致迭代后的大部分$\theta$参数$\approx$0，最后模型输出$\approx$0，使之欠拟合；

$\lambda$值过小，导致过拟合现象；

解决方案：
#### 自动选择

1. $\lambda=0$
2. $\lambda=0.01$
3. $\lambda=0.02$
4. $\lambda=0.04$
5. $\lambda=0.08$
.<br>
.<br>
.<br>
12. $\lambda=10$

用训练集批次$\mathop{min}limits_{\theta}J(\theta)$，然后通过验证集批次计算各自的$J_{cv}(\theta^(i))$平均的误差平方和，接着选择误差值最小的$\lambda$值，做完后观察该模型在测试集的泛化能力表现



#### 手动选择

偏差/方差作为正则化参数$\lambda$，自己手动调整$\lambda$值(0.01~1000)，观察训练误差和验证误差，然后从验证集的曲线低洼处选择最合适$\lambda$，最后查验模型的泛化能力。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/3.png)

### 学习曲线

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/4.png)

随着m个样本增加，训练误差随m增大二增大，验证误差经较少的样本m训练后，误差项较大，随m增大而减少，直至贴近训练误差变平。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/5.png)

因为模型是直线，不能很好拟合数据，m增加时总误差项增大，到了最后变平。所以样本增加的再多，训练误差不会有什么变化，如左边的紫线和蓝线随着m增大，结果没什么太大变化。

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/6.png)

总误差项随着样本m增加而增加，但和验证误差的曲线间隔较大，说明方差较高，可以通过增加样本或正则化来解决。

### 评价学习算法

调试一个学习算法:
假如已实现正则线性回归模型来预测房价。然而，用一组新的样本来测试效果，发现它在预测中出现了难以接受的巨大错误。接下来要做的判断：
（先画学习曲线）
- 获取更多训练样本              解决高方差问题
- 减少特征数量                  解决高方差问题
- 添加更多特征                  解决高偏差问题
- 减少$\lambda$值               解决高偏差问题
- 增加$\lambda$值               解决高方差问题

![](https://github.com/allrobot/Study-Blog/raw/main/assets/images/2022-02-17/7.png)

神经网络规模较小、参数少、容易欠拟合，计算量成本较低

神经网络规模较大、参数多、容易过拟合，计算量成本较高

默认一层隐藏层神经网络，添加隐藏层需测试验证集的误差，关于过拟合用正则化解决。

## 快速开发机器学习

开局先用简单粗暴的算法训练参数，绘制学习曲线，观察图进行误差分析，根据数据评价指标了解哪些算法是需要或不需要的，以及训练集、参数都有哪些问题等等，从而决定之后的优化方法

### 误差度量评估算法

癌症分类示例：
训练逻辑回归模型$h_{\theta}(x)$(癌症y=1，否则y=0)，当发现模型在测试集中有1%的错误。

测试集中有0.5%人有癌症，这错误率就不那么好了

假如模型的错误率降低到0.8，甚至0.5%，模型的质量是否提升？

需要一个标准来判定：

<table>
	<tr style="border: none;">
		<th style="border: none;"></th>
		<th style="border: none;"></th>
		<th style="border: none;">实际</th>
		<th style="border: none;">分组</th>
	</tr>
	<tr style="border: none;">
		<th style="border: none;"></th>
		<th style="border: none;"></th>
		<th style="border: none;">1</th>
		<th style="border: none;">0</th>
	</tr>
	<tr>
		<th style="border: none;">预测</th>
		<th style="border: none;">1</th>
		<th>真阳性</th>
		<th>假阳性</th>
	</tr>
	<tr>
		<th style="border: none;">分类</th>
		<th style="border: none;">0</th>
		<th>假阴性</th>
		<th>真阴性</th>
	</tr>
</table>

精度：
所有病人中预测y=1的，有多少人换癌症？

查准率：在所有预测患有癌症的患者中有多大比率的病人是真正拥有癌症的

$$
\begin{equation*} 

查准率=\frac{所有预测正确有阳性的人(真阳性)}{所有预测有阳性的人}=\frac{真阳性}{真阳性+假阳性}

\end{equation*}
$$

召回率：在所有患者中真实有癌症的人有多大比率


$$
\begin{equation*} 

召回率=\frac{真阳性}{所有患有癌症的人}=\frac{真阳性}{真阳性+假阴性}

\end{equation*}
$$

好的模型，查准率和召回率很高


























