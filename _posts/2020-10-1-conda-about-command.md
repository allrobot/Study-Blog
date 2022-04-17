---
layout: article
title:  "conda常用指令"
date:   2022-02-23
categories: Anaconda
tags: [Anaconda,python,enviroment]
---

<!-- ./images/2022-3-9/ 
$\displaystyle\underbrace{a_i}_{\text{i从1到n}}$

$\displaystyle\mathop{a_i}\limits_{i\text{从1到}n}$
-->

## 镜像源
切回官方源
```
conda config --remove-key channels
```
个人使用的源

conda config --add channels XXXX
conda config --set ssl_verify false
```
ssl_verify: true
show_channel_urls: true


channels:
  - https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
  
  - https://mirrors.aliyun.com/anaconda/pkgs/
  - https://mirrors.aliyun.com/anaconda/miniconda/
  - https://mirrors.aliyun.com/anaconda/cloud/
  - https://mirrors.aliyun.com/anaconda/archive/
  
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/win-64/
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/win-64/
  - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/win-64/
```

## 虚拟环境相关操作
常用命令
```console
conda env list                                        # 或 conda info -e 查看当前存在哪些虚拟环境
conda update conda                                    # 检查更新当前conda
conda upgrade --all                                   # 升级全部库
conda update packagename                              # 升级一个包
conda install packagename                             # 安装包
conda installl numpy pandas scipy                     # 安装多个包
conda install numpy =1.10                             # 安装固定版本的包
conda remove packagename                              # 移除一个包
conda list                                            # 查看所有包

conda create -n env_name list of packagenaem          # 创建虚拟环境
conda create -n env_name pandas 
conda create -n env_name python2 = 2.7 pandas         # 指定python版本
activate env_name                                     # 激活环境
deactivate  env_name                                  # 退出环境
conda env remove -n env_name                          # 删除虚拟环境
conda env list                                        # 显示所有虚拟环境
```
>对于Linux，虚拟环境操作相关的前面需加上conda，虚拟环境激活source activate xxx

1. 创建虚拟环境以及指定的python版本
```console
conda create -n xxx python=3.6
```
3. 使用激活(或切换不同python版本)的虚拟环境
```console
python --version                                       # 可以检查当前python的版本
Linux:  source activate your_env_name(虚拟环境名称)    # 激活虚拟环境
Windows: activate your_env_name(虚拟环境名称)
```
xxx为自己命名的虚拟环境名称，该文件可在Anaconda安装目录 envs文件下找到
4. 退出当前虚拟环境
```console
Linux: source deactivate
​
Windows:conda deactivate                                # 退出当前虚拟环境
```

5. 删除虚拟环境
```console
conda remove -n your_env_name(虚拟环境名称) --all       # 删除虚拟环境所有的包
conda remove --name your_env_name  package_name         # 删除环境中的某个包
```



## 对虚拟环境中安装额外的包
```console
conda install -n your_env_name [package]
```

## 安装本地包
```console
conda install -c file:///驱动符号/xx/xx.whl
conda install -c local xxx
conda install  --use-local xxx
```
## 安装包相关命令
```console
conda install xxxxx==version                                  # 安装特定版本的包
conda update xxxxx==version                                   # 升级到某版本的包
conda search xxxxx                                            # 搜索包
conda upgrade -all                                            # 更新所有包
pip install -i http://pypi.douban.com/simple xxxx             # pip安装指定源
```
## 更新conda
conda安装某包时遇到奇奇怪怪的问题，使用下面命令

```console
conda deactivate                                              # 更新前先关掉虚拟环境
conda update --name base conda
conda update --all

# 无法
conda update anaconda
conda install anaconda
conda activate base
conda update conda -n root              
conda update -n base -c defaults conda
```

PIP
```
python -m pip install --upgrade pip
```


## 导出/导入环境
```console
conda env export --file environment.yaml //导出环境配置
conda list --explicit > environment.txt
conda list -e > environment.txt

conda env create -f environment.yaml     //导入环境配置
conda create --name <environment-name> --file environment.txt
conda create -n <environment-name> --file environment.txt
```
在已有的虚拟环境更新特定的包
```
conda activate <environment-name>                               //进入虚拟环境
conda env update --file environment.yml --prune    //更新特定的包
```
或者
```
conda env update --name <environment-name> --file environment.yml --prune
```

[更多更新指南](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html?highlight=prune#updating-an-environment)

## 删除缓存
```console
conda clean -p      //删除所有剩下的包
conda clean -t      //tar打包
conda clean -y --all //删除所有剩下的安装包及cache
conda clean -i      //清除索引缓存
```

```console
anaconda search -t conda xxxxx            //搜索包
anaconda show xxxxx                       //显示xxx包的信息及下载地址
conda install --channel https://xxxx xxx  // 指定某源的安装包
conda install -c https://xxxxxx xxx       //同上
```

## Pycharm
shift+enter                  向下插入新的一行
ctrl+shift+enter             向上插入新的一行
ctrl+鼠标                    进入函数的定义文档
ctrl+空格                    进入建议补全列表
ctrl+q                       快速查看当前函数帮助文档