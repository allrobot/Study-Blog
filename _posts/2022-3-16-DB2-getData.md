---
layout: article
title:  "S1_E1_A1数据处理"
date:   2022-03-16
categories: 机器学习
tags: [机器学习,sEMG,数据处理]
---
## 保存或加载模型

```python

```

## DB2数据格式

DB2肌电数据集，信号以2KHZ速率进行采样，受试者重复49种不同动作
每个动作持续5秒，休息间隔3秒

acc包含36列，12个电极的三轴加速计
emg为12列，12个电极的sEMG信号

```python
'''
DB2肌电数据集，信号以2KHZ速率进行采样，受试者重复49种不同动作
每个动作持续5秒，休息间隔3秒

acc包含36列，12个电极的三轴加速计
emg为12列，12个电极的sEMG信号
'''
	np.set_printoptions(suppress=True)
    E1 = scio.loadmat('S1_E1_A1.mat')

    print(E1.keys())

    E1_emg = E1['emg']
    print(type(E1_emg))
    print(E1_emg.shape)
    print(E1_emg)
    print(E1_emg[0])

    E1_label = E1['restimulus']
    print(type(E1_label))
    print(E1_label.shape)
    print(E1_label)
    print(E1_label[0])

    # 识别活动段
    index1 = []
    for i in range(len(E1_label)):
        if E1_label[i] != 0:
            index1.append(i)
    label1 = E1_label[index1, :]
    emg1 = E1_emg[index1, :]

    print(type(label1))
    print(label1.shape)
    print(label1[0])
    print(type(emg1))
    print(emg1.shape)
    print(emg1)
    print(emg1[0])

    emg = np.vstack((emg1))
    print(type(emg))
    print(emg.shape)
    print(emg)
    print(emg[0])
    label = np.vstack((label1))
    print(label.shape)
    print(label)
    print(label[0])
```

返回

```bash
# 输出EB2的都有哪些数据
dict_keys(['__header__', '__version__', '__globals__', 'subject', 'exercise', 'emg', 'acc', 'gyro', 'mag', 'glove', 'stimulus', 'repetition', 'restimulus', 'rerepetition'])
# 输出的E1['emg']为ndarray格式
<class 'numpy.ndarray'>
# emg原始数据，姑且视为16个传感器的数值
(2292526, 16)
[[-0.00001221  0.00001285  0.00000142 ...  0.00000245 -0.00000036
   0.00000399]
 [-0.00001103  0.00001196 -0.00000081 ...  0.00000758 -0.00000008
   0.00000972]
 [-0.00000056  0.00001026 -0.0000009  ...  0.00000975 -0.00000255
   0.00000637]
 ...
 [-0.00000351 -0.0000128   0.00000133 ... -0.00000035 -0.0000023
   0.00000127]
 [-0.00000092 -0.0000144   0.00000039 ... -0.00000188 -0.00000281
   0.00000013]
 [ 0.00000009 -0.00001315 -0.0000002  ... -0.00000232 -0.00000334
   0.00000011]]
[-0.00001221  0.00001285  0.00000142 -0.00000145  0.00000089  0.00000128
 -0.00000432 -0.0000021  -0.00001083 -0.00000186 -0.000001   -0.00000192
  0.00000232  0.00000245 -0.00000036  0.00000399]
# 给每段emg打标签的，分别为0~8
<class 'numpy.ndarray'>
(2292526, 1)
[[0]
 [0]
 [0]
 ...
 [0]
 [0]
 [0]]
# 标签[0]
[0]
# 把0的标签以及对应的emg去掉
<class 'numpy.ndarray'>
# 2292526个sEMG数据段减少至1104351个
(1104351, 1)
[1]
<class 'numpy.ndarray'>
(1104351, 16)
[[-0.00003091  0.00001542  0.0000059  ... -0.00001223 -0.00000005
   0.00000929]
 [-0.00001082  0.00001816  0.00000737 ... -0.00001045 -0.0000023
   0.00000329]
 [-0.00000224  0.00002049  0.00000749 ... -0.00002109 -0.00000125
   0.0000015 ]
 ...
 [ 0.00003119  0.00002206  0.00000496 ...  0.00000825  0.00000369
  -0.00000032]
 [ 0.00002186  0.00001378  0.00000433 ...  0.00001304  0.00000322
   0.00000338]
 [ 0.00002182  0.00002022  0.00000433 ...  0.00001906  0.00000615
  -0.00000058]]
[-0.00003091  0.00001542  0.0000059   0.00000115 -0.00000116  0.00000269
  0.00000543 -0.00000343  0.00000074 -0.00001227 -0.00000229 -0.0000013
 -0.00000389 -0.00001223 -0.00000005  0.00000929]
<class 'numpy.ndarray'>
(1104351, 16)
[[-0.00003091  0.00001542  0.0000059  ... -0.00001223 -0.00000005
   0.00000929]
 [-0.00001082  0.00001816  0.00000737 ... -0.00001045 -0.0000023
   0.00000329]
 [-0.00000224  0.00002049  0.00000749 ... -0.00002109 -0.00000125
   0.0000015 ]
 ...
 [ 0.00003119  0.00002206  0.00000496 ...  0.00000825  0.00000369
  -0.00000032]
 [ 0.00002186  0.00001378  0.00000433 ...  0.00001304  0.00000322
   0.00000338]
 [ 0.00002182  0.00002022  0.00000433 ...  0.00001906  0.00000615
  -0.00000058]]
[-0.00003091  0.00001542  0.0000059   0.00000115 -0.00000116  0.00000269
  0.00000543 -0.00000343  0.00000074 -0.00001227 -0.00000229 -0.0000013
 -0.00000389 -0.00001223 -0.00000005  0.00000929]
(1104351, 1)
# 输出标签1~9，共9个
[[1]
 [1]
 [1]
 ...
 [9]
 [9]
 [9]]
[1]


进程已结束,退出代码0
```

## sEMG曲线绘图

该数据的原始值经过缩放，需科学计数法才能显示

把emg的数放大200000倍

![](./assets/images/2022-3-16/1.png)

```PYTHON
    E1 = scio.loadmat('S1_E1_A1.mat')
    E1_emg = E1['emg']
    E1_label = E1['restimulus']
    plt.plot(E1_emg[0:50000] * 200000)
    plt.plot(E1_label[0:50000]*100)
    plt.show
```
![](./assets/images/2022-3-16/2.png)

### 截取肌电并输出

`E1_emg[0:5]`输出6个肌电段：
\[\[-0.00001221  0.00001285  0.00000142 -0.00000145  0.00000089  0.00000128
  -0.00000432 -0.0000021  -0.00001083 -0.00000186 -0.000001   -0.00000192
   0.00000232  0.00000245 -0.00000036  0.00000399]
 \[-0.00001103  0.00001196 -0.00000081 -0.00000296  0.00000162  0.00000013
  -0.00000231 -0.0000021  -0.00000726 -0.00000127  0.00000254 -0.00000042
   0.00000163  0.00000758 -0.00000008  0.00000972]
 \[-0.00000056  0.00001026 -0.0000009  -0.00000081  0.00000148 -0.00000357
  -0.00000178 -0.00000029 -0.00000974  0.00000537  0.00000619  0.00000318
  -0.00000306  0.00000975 -0.00000255  0.00000637]
 \[ 0.00000486  0.00000656 -0.00000097  0.00000017  0.00000206 -0.00000565
  -0.00000334  0.00000056 -0.00001076  0.00000742  0.0000053   0.00000458
  -0.00000487  0.00000957 -0.00000351  0.00000383]
 \[ 0.00000319  0.00000352 -0.00000115  0.00000071  0.00000601 -0.0000052
  -0.0000062  -0.00000028 -0.00001131  0.00000479 -0.00000075  0.00000377
  -0.00000339  0.00000709 -0.00000234  0.00000274]]
使用`E1_emg[0:5,2]`输出连续肌电段的第3个传感器的值：
\[ 0.00000142 -0.00000081 -0.0000009  -0.00000097 -0.00000115]

## 获取处理好的sEMG数据

```python
import scipy.io as scio
import matplotlib.pyplot as plt
import numpy as np
import math
from sklearn.preprocessing import MinMaxScaler
import h5py
from feature import *
import math
'''
DB2肌电数据集，信号以2KHZ速率进行采样，受试者重复49种不同动作
每个动作持续5秒，休息间隔3秒

acc包含36列，12个电极的三轴加速计
emg为12列，12个电极的sEMG信号
'''
np.set_printoptions(suppress=True)
def getdata():
    E1 = scio.loadmat('S1_E1_A1.mat')
    print(E1.keys())
    E1_emg = E1['emg']
    E1_label = E1['restimulus']
    
    # 提取活动段
    index1 = []
    for i in range(len(E1_label)):
        if E1_label[i] != 0:
            index1.append(i)
    label1 = E1_label[index1, :]
    emg1 = E1_emg[index1, :]


    emg = np.vstack((emg1))
    label = np.vstack((label1))
    label = label - 1

    # plt.plot(E1_emg[0:200000] * 20000)
    # plt.plot(E1_label[0:200000]*100)

    featureData = []
    featureLabel = []
    classes = 9
    timeWindow = 200
    strideWindow = 200

    # 1~9标签，把各个分类的emg段添加到到iemg，然后特征处理
    for i in range(classes):
        index = []
        for j in range(label.shape[0]):
            if (label[j, :] == i):
                index.append(j)
        iemg = emg[index, :]
        # 取整数
        np.set_printoptions(threshold=np.inf)
        f=open('test.txt','w')
        f.write(str(iemg))
        f.close()
        np.set_printoptions(threshold=False)
        length = math.floor((iemg.shape[0] - timeWindow) / strideWindow)
        print("class ", i, ",number of sample: ", iemg.shape[0], length)

        for j in range(length):
            rms = featureRMS(iemg[strideWindow * j:strideWindow * j + timeWindow, :])
            mav = featureMAV(iemg[strideWindow * j:strideWindow * j + timeWindow, :])
            wl = featureWL(iemg[strideWindow * j:strideWindow * j + timeWindow, :])
            zc = featureZC(iemg[strideWindow * j:strideWindow * j + timeWindow, :])
            ssc = featureSSC(iemg[strideWindow * j:strideWindow * j + timeWindow, :])
            featureStack = np.hstack((rms, mav, wl, zc, ssc))

            featureData.append(featureStack)
            featureLabel.append(i)
    featureData = np.array(featureData)

    print(featureData.shape)
    print(len(featureLabel))

    emg = emg * 20000

    imageData = []
    imageLabel = []
    imageLength = 200
    classes = 9

    for i in range(classes):
        index = []
        for j in range(label.shape[0]):
            if (label[j, :] == i):
                index.append(j)

        iemg = emg[index, :]
        length = math.floor((iemg.shape[0] - imageLength) / imageLength)
        print("class ", i, " number of sample: ", iemg.shape[0], length)

        for j in range(length):
            subImage = iemg[imageLength * j:imageLength * (j + 1), :]
            imageData.append(subImage)
            imageLabel.append(i)

    imageData = np.array(imageData)
    print(imageData.shape)
    print(len(imageLabel))

    file = h5py.File('DB2//DB2_S1_feature_200_0.h5', 'w')
    file.create_dataset('featureData', data=featureData)
    file.create_dataset('featureLabel', data=featureLabel)
    file.close()

    file = h5py.File('DB2//DB2_S1_image_200_0.h5', 'w')
    file.create_dataset('imageData', data=imageData)
    file.create_dataset('imageLabel', data=imageLabel)
    file.close()
```

返回
```BASH
C:\Users\Administrator\.conda\envs\tf-gpu\python.exe C:/Users/Administrator/PycharmProjects/Request_Project/main.py
dict_keys(['__header__', '__version__', '__globals__', 'subject', 'exercise', 'emg', 'acc', 'gyro', 'mag', 'glove', 'stimulus', 'repetition', 'restimulus', 'rerepetition'])
class  0 ,number of sample:  82564 411
class  1 ,number of sample:  93842 468
class  2 ,number of sample:  160468 801
class  3 ,number of sample:  120850 603
class  4 ,number of sample:  112737 562
class  5 ,number of sample:  88592 441
class  6 ,number of sample:  175325 875
class  7 ,number of sample:  120336 600
class  8 ,number of sample:  149637 747
(5508, 80)
5508
class  0  number of sample:  82564 411
class  1  number of sample:  93842 468
class  2  number of sample:  160468 801
class  3  number of sample:  120850 603
class  4  number of sample:  112737 562
class  5  number of sample:  88592 441
class  6  number of sample:  175325 875
class  7  number of sample:  120336 600
class  8  number of sample:  149637 747
(5508, 200, 16)
5508

进程已结束,退出代码0

```

## 输出预测值

```PYTHON
f=h5py.File('DB2_S1_image_200_0.h5','r')
image=f['imageData'][:]
print(image.shape)
label=f['imageLabel'][:]
print(label.shape)
f.close()

data=np.expand_dims(image,axis=3)
label=np.eye(9)[label.reshape(-1)]

X=data[0,:,:,:].reshape(-1,200,16,1)
Y=label[0,:]
print(X.shape)
print(Y.shape)
model=load_model('DB2_Conv2D_model.h5')
model.load_weights('DB2_Conv2D_model.hdf5')

np.set_printoptions(suppress=True)
m=model.predict(X)
print(m)
print(np.argmax(m,axis=1))
print(Y)

```

### 存储

```PYTHON
f=h5py.File('DB2_S1_Image_[200_16_1].h5','w')
f.create_dataset('imageData',data=X)
f.create_dataset('imageLabel',data=Y)
print(f['imageData'].shape)
print(f['imageLabel'].shape)
f.close()
```
返回`(5508, 200, 16, 1)`和`(5508, 9)`
