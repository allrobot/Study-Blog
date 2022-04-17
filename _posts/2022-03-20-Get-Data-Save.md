---
layout: article
title:  "数据处理保存到h5py"
date:   2022-03-20
categories: 机器学习
tags: [机器学习,sEMG,数据处理]
---

## 提取txt保存到h5py中

```python
file=h5py.File("EMG.h5",'w')
data=[]

# emg为文件夹，提取所有路径，该路径有若干xxx.txt
for i in os.listdir('emg'):
    path="emg/"+i
    with open(path, 'r') as f:
        for line in f.readlines():
            line = list(map(int, line.strip('\n').split(',')))
            if (len(line) == 6):
                data.append(line)
    file[i[:-4]] = data
    data.clear()
    print(file.get(i[:-4]))
print(file.keys())
file.close()
```