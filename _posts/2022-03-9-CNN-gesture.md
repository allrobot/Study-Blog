---
layout: article
title:  "CNN识别手势的分类器"
date:   2022-02-19
categories: 机器学习
tags: [机器学习,sEMG,分类器]
---

<!-- ./images/2022-3-9/ 
$\displaystyle\underbrace{a_i}_{\text{i从1到n}}$

$\displaystyle\mathop{a_i}\limits_{i\text{从1到}n}$
-->

## 分类器

使用的是CNN图像识别分类器，把预处理后的sEMG信号按滑动窗口分割一段段表示活动段信号的数据集。
### 数据预处理
数据集来源于NinaPro-DB1，该数据集的sEMG信号已经滤波去噪处理，打好标签：

![打标签](./images/2022-3-9/2022-3-9/1.png)

该数据集采用的是10通道传感器（10个传感器，分别为10个信号源），每个信号源的电压值合计小于`阈值±一定范围值`则识别为休息状态，若**高于阈值**则识别为活动段信号打标签。

整理成数据集后，所有的特征采用**特征归一化**便于加快模型收敛速度

数据集包含53种手势，图像格式为120ms\*10通道，120ms为滑动窗口，如图所示：

![](./images/2022-3-9/2.png)

### 网络模型

![](./images/2022-3-9/3.png)