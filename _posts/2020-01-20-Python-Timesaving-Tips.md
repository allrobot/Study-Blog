---
layout: article
title:  "python节约时间小技巧"
date:   2020-01-20
categories: python
tags: [python]
---

### 字符串反转

　　使用切片反转字符串。

　　str1="qwert" rev_str1=str1[::-1] #输出 # trewq

### 使首字母大写

　　将字符串转换为首字母大写。使用 title()方法完成的。

　　str1="this is a book" print(str1.title()) # This Is A Book

### 在字符串中查找唯一元素

　　下面代码可用于查找字符串中所有的唯一元素。

　　str1="aabbccccdddd"set1=set(str1) new_str=''.join(set1) print(new_str)

### 重复打印字符串或列表

　　下面的代码中，对字符串或列表使用(*)。把字符串或列表复制多次。

　　i=4 str1="abcd" list1=[1,2] print(str1*i) # abcdabcdabcdabcd print(list1*i) # [1,2,1,2,1,2,1,2]

### 列表推导式

　　列表推导式为我们提供了一种在其他列表基础上创建列表的好方法。下面代码通过将旧列表的每个元素乘以 2 来创建新列表。

　　list1=[1,2,3] new_list1=[2*i for i in list1] # [2,4,6]

### 交换变量

　　不使用另一个变量，实现变量交换。

　　x=1 y=2 x,y=y,x print(x) # 2 print(y) # 1

### 将字符串拆分为子字符串列表

　　我们使用字符串类中的.split()方法将字符串拆分为子字符串列表，还可以将要分割的分隔符作为参数传递。

　　str1="This is a book"str2="test/ str 2"print(str1.split()) # ['This', 'is', 'a', 'book'] print(str2.split('/')) # ['test', ' str 2']

### 将字符串列表组合成单个字符串

　　join()将作为参数传递的字符串列表组合为单个字符串。这种情况下，我们使用逗号分隔符将它们分开。

　　list_str=['This','is','a','book']print(','.join(list_str))# This,is,a,book

### 检查回文字符串

　　我们已经讨论过如何反转字符串，因此回文字符串在 Python 中判断起来非常简单。

　　str1="qqaabb"if str1==str1[::-1]: print("回文")else: print("不是") # 不是

### 列表中的元素统计

　　使用 Python Counter 类。Python 计数器跟踪容器中每个元素的频数， Counter()返回一个字典，元素作为键，频数作为值。

　　另外使用 most_common()函数来获取列表中的 出现次数最多的元素。

　　from collections import Counterlist1=['a','b','a','c','c','c']count=Counter(list1)print(count)print(count['b'])print(count.most_common(1))

### 判断两个字符串是否为异序词

　　异序词是通过重新排列不同单词或短语的字母而形成的单词或短语。如果两个字符串的 Counter 对象相等，那么它们就是相同字母异序词对。

　　s1,s2,s3="acbde","abced","abcda"c1,c2,c3=Counter(s1),Counter(s2),Counter(s3)if c1==c2: print('1和2是异序词') if c1==c3: print('1和3是异序词')

### 使用 try-except-else 块

　　try / except 是 Python 中的异常处理模块，添加 else 语句，会在 try 块中没有引发异常的情况下运行。

　　a,b=1,0try: print(a/b) # b为0的时候触发异常except ZeroDivisionError: print("除数为0")else: print("不存在异常")finally: print("此段总是会执行")

### 通过枚举获取索引 / 值对

　　可以使用下面的脚本，遍历列表中的值及其索引。

　　list1=['a','b','c','d','e']for idx,val in enumerate(list1): print('{0}:{1}'.format(idx,val))# 0:a# 1:b# 2:c# 3:d# 4:e

### 获取对象的内存使用信息

　　下面脚本可用于检查对象的内存使用信息。

　　import sysnum=21print(sys.getsizeof(num))

### 合并两个字典

　　在 Python 2 中，使用 update()合并两个字典，Python 3 变得更加简单。

　　下面脚本中，两个字典被合并。在相交的情况下，使用第二个字典中的值。

　　dic1={'app':9,'banana':6}dic2={'banana':4,'orange':8}com_dict={**dic1,**dic2}# {'apple':9,'banana':4,'orange':8}

### 计算代码执行所需的时间

　　下面代码使用库函数来计算执行代码所需的时间消耗多少毫秒。

　　import times_time=time.time()a,b=1,2c=a+b e_time=time.time()time_taken_in_micro=(e_time-stime)*(10**6)print("程序运行的毫秒：{0} ms".format(time_taken_in_micro))

### 展开列表清单

　　有时不知道列表的嵌套深度，并且只想把所有元素放在一个普通列表中。可以通下面的方法得到数据：

　　from iteration_utilities import deepflatten# 如果嵌套列表的深度只有1层def flatten(l): return [item for sublist in l for item in sublist]l=[[1,2,3],[3]]print(flatten(l))# [1,2,3,3]# 如果不知道列表嵌套深度l=[[1,2,3],[4,[5],[6,7]],[8,[9,[10]]]]print(list(deepflatten(l,depth=3)))# [1,2,3,4,5,6,7,8,9,10]

### 从列表中随机取样

　　下面代码从给定列表中生成了 n 个随机样本。

　　import randomlist1=['a','b','c','d','e']ns=2samples=random.sample(list1,ns)print(samples)# ['a','c']

　　或者使用secrets库生成随机样本进行， 下面代码仅适用于 Python 3.x。

　　import secretss_rand=secrets.SystemRanom()list1=['a','b','c','d','e']ns=2samples=s_rand.sample(list1,ns)print(samples)# ['c','d']

### 数字列表化

　　下面代码将整数转换为数字列表。

　　nums=123456# 使用mapdigit_list=list(map(int,str(nums)))print(digit_list)# [1,2,3,4,5,6]# 使用列表表达式digit_list=[int(x) for x in str(nums)]print(digit_list)# [1,2,3,4,5,6]

### 唯一性检查

　　下面的函数检查列表中的元素是否唯一。

　　def unique(l): if len(l)==len(set(l)): print("所有元素是唯一的") else: print("存在重复") unique([1,2,3,4]) # 所有元素是唯一的 unique([1,1,3,4]) # 存在重复