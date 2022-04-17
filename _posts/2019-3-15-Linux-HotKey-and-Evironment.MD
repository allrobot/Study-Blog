---
layout: article
title:  "Linux常用快捷键及环境变量配置"
date:   2019-3-15
categories: Linux
tags: [Linux,环境变量,常用命令]
---

## python环境包
windows:conda create -n xxx python=3.7 tensorflow-gpu h5py pandas matplotlib 

linux:conda create -n xxx python=3.7 tensorflow-gpu h5py pandas matplotlib pybluez

## Linux

Linux 命令行编辑快捷键
初学者在Linux命令窗口（终端）敲命令时，肯定觉得通过输入一串一串的字符的方式来控制计算是效率很低。 但是Linux命令解释器（Shell）是有很多快捷键的，熟练掌握可以极大的提高操作效率。 下面列出最常用的快捷键，这还不是完全版。

命令行快捷键：
常用：

Ctrl L ：清屏
Ctrl M ：等效于回车
Ctrl C : 中断正在当前正在执行的程序
历史命令：

Ctrl P : 上一条命令，可以一直按表示一直往前翻
Ctrl N : 下一条命令
Ctrl R，再按历史命令中出现过的字符串：按字符串寻找历史命令（重度推荐）
命令行编辑：

Tab : 自动补齐（重度推荐）
Ctrl A ： 移动光标到命令行首
Ctrl E : 移动光标到命令行尾
Ctrl B : 光标后退
Ctrl F : 光标前进
Alt F : 光标前进一个单词
Alt B : 光标后退一格单词
Ctrl ] : 从当前光标往后搜索字符串，用于快速移动到该字符串
Ctrl Alt ] : 从当前光标往前搜索字符串，用于快速移动到该字符串
Ctrl H : 删除光标的前一个字符
Ctrl D : 删除当前光标所在字符
Ctrl K ：删除光标之后所有字符
Ctrl U : 清空当前键入的命令
Ctrl W : 删除光标前的单词(Word, 不包含空格的字符串)
**Ctrl \ ** : 删除光标前的所有空白字符
Ctrl Y : 粘贴Ctrl W或Ctrl K删除的内容
Alt . : 粘贴上一条命令的最后一个参数（很有用）
Alt [0-9] Alt . 粘贴上一条命令的第[0-9]个参数
Alt [0-9] Alt . Alt. 粘贴上上一条命令的第[0-9]个参数
Ctrl X Ctrl E : 调出系统默认编辑器编辑当前输入的命令，退出编辑器时，命令执行
其他：

Ctrl Z : 把当前进程放到后台（之后可用''fg''命令回到前台）
Shift Insert : 粘贴（相当于Windows的Ctrl V）
在命令行窗口选中即复制
在命令行窗口中键即粘贴，可用Shift Insert代替
Ctrl PageUp : 屏幕输出向上翻页
Ctrl PageDown : 屏幕输出向下翻页

```BASH
nano xxx.c
# ctrl+xxx进行操作，ctrl+O保存,ctrl+X退出
```

Ubuntu安装源：
```BASH
vim /etc/apt/sources.list

# 不要进编辑模式，按ggVD清空全部链接，然后插入

deb https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
sudo apt update
```

conda安装源：
```BASH
conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/

conda config --set auto_activate_base false
```

若安装别的或提示仓库xxxx没有xxx文件，则删掉该仓库链接
```BASH
sudo add-apt-repository --remove ppa:/xx
# 示例
# sudo add-apt-repository --remove ppa:/webupd8team/sublime-text-3
```

```
conda config --add channels 

sudo add-apt-repository --remove ppa:/xx



```bash
# 打开环境变量配置文件
vim /etc/enviroment

# 环境变量配置
export xxx1=/xxx/xxx_path
export PATH=$xxx1/bin:$PATH

export xxx2=/xxx/xxx_path
export xxx3=$xxx2/xxx
export PATH_1=$xxx3/bin:$PATH
# 然后按Esc，输入wq保存并且退出

#加载环境配置
source /etc/enviroment

# 检查是否正常
echo $xxx1
echo $xxx2
```
环境变量读取顺序
```bash
# 对所有用户生效，永久生效
/etc/environment
/etc/profile
/etc/bashrc
# 对当前用户有效，永久生效
~/.bash_profile
~/.profile
~/.bashrc
```


