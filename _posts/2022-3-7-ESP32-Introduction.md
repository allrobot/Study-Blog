---
layout: article
title:  "ESP32入门"
date:   2022-03-20
categories: 硬件开发
tags: [ESP32,BT,硬件开发]
---

## 两种ESP开发方式

开发方式有很多，比如PlatformIO、VSCode之流，亦可在Linux、MAC平台开发，选自己喜欢的开发方式就行

### ESP-IDF
4.4版本的ESP-IDF编程指南:<https://docs.espressif.com/projects/esp-idf/zh_CN/v4.4/esp32/get-started/index.html>
#### CMD

一个是用CMD命令行进行构建项目并且编译烧录到固件

下载链接：<https://dl.espressif.com/dl/esp-idf/>

只下离线版安装包，下载完毕执行安装

确保使用安装包内的python路径添加到path
添加目录格式：

`C:\XXXXXX\.ESPRESSIF\TOOLS\IDF-PYTHON\3.8.7`

和

`C:\XXXXXX\.ESPRESSIF\TOOLS\IDF-PYTHON\3.8.7\Scripts`

否则，CMD报错找不到模块xxx

打开“ESP-IDF 4.4 CMD”窗口，窗口会提示：`No directories added to PATH:`

下面缺失的目录一块儿复制到Notepad++把`换行`换成`;`，添加到path即可，注意path内的tool路径需靠前，改成高优先级

关掉“ESP-IDF 4.4 CMD”窗口再打开，如果提示
```
Active code page: 65001
Setting PYTHONNOUSERSITE, was not set
Using Python in C:\Users\XXXX\esp\.espressif\python_env\idf4.4_py3.8_env\Scripts\
Python 3.8.7
Using Git in C:\Users\XXXX\esp\.espressif\tools\idf-git\2.34.2\cmd\
git version 2.34.1.windows.1
Setting IDF_PATH: C:\Users\XXXX\esp\.espressif\frameworks\esp-idf-v4.4

Adding ESP-IDF tools to PATH...
No directories added to PATH:

XXXXXXXXXXXXXXXXXXXXXXXXXXXXX这里一大堆路径;

Checking if Python packages are up to date...
Python requirements from C:\Users\XXXX\esp\.espressif\frameworks\esp-idf-v4.4\requirements.txt are satisfied.

Done! You can now compile ESP-IDF projects.
Go to the project directory and run:

  idf.py build
```
就OK了

#### 构建编译项目

复制`C:\Users\XXXX\esp\.espressif\frameworks\esp-idf-v4.4\examples\get-started\`的`hello_world`文件夹到任意目录，CMD跳到`hello_world`执行以下指令：

- `idf.py set-target esp32`
- `idf.py menuconfig`

`idf.py set-target esp32`的`esp32`为目标板块。
`idf.py menuconfig`配置项目

开始构建项目

- `idf.py build`

运行以上命令可以编译应用程序和所有 ESP-IDF 组件，接着生成 bootloader、分区表和应用程序二进制文件。

```
$ idf.py build
Running cmake in directory /path/to/hello_world/build
Executing "cmake -G Ninja --warn-uninitialized /path/to/hello_world"...
Warn about uninitialized values.
-- Found Git:/usr/bin/git (found version "2.17.0")
-- Building empty aws_iot component due to configuration
-- Component names: ...
-- Component paths: ...

... (more lines of build system output)

[527/527] Generating hello_world.bin
esptool.py v2.3.1

Project build complete. To flash, run this command:
../../../components/esptool_py/esptool/esptool.py -p (PORT) -b 921600 write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x10000 build/hello_world.bin  build 0x1000 build/bootloader/bootloader.bin 0x8000 build/partition_table/partition-table.bin
or run 'idf.py -p PORT flash'
```

#### 烧录

使用以下命令，将刚刚生成的二进制文件 (bootloader.bin, partition-table.bin 和 hello_world.bin) 烧录至 ESP32 开发板：

`idf.py -p COM5 flash`

波特率`idf.py -p PORT [-b BAUD] flash`设置其中的BAUD即可

默认波特率为 460800。

例如：`idf.py -p COM5 -b 115200 flash`

>勾选 flash 选项将自动编译并烧录工程，因此无需再运行 idf.py build。

允许执行多个指令，格式如`idf.py -p <your-board-serial-port> flash monitor`

#### 示例文件

`C:\Users\XXXX\esp\.espressif\frameworks\esp-idf-v4.4\examples`

### Arduino

在Arduino平台开发ESP的编程指南：<https://docs.espressif.com/projects/arduino-esp32/en/latest/getting_started.html>

Github：<https://github.com/espressif/arduino-esp32#readme>

#### 开发板json

[开发板参考链接](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html#installing-using-arduino-ide)

稳定版链接：

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

首选项填入第三方包的链接，然后“工具”-->“开发板”-->“开发板管理器”选择ESP即可

![](https://docs.espressif.com/projects/arduino-esp32/en/latest/_images/install_guide_boards_manager_esp32.png)

开发板管理器的ESP32作者为`Espressif Systems`

#### 示例文件

Arduino IDE-->工具-->开发板-->目标开发板的名称，选择要开发的板子，然后选 Arduino IDE-->文件-->示例-->xxx开发板-->任意示例文件

打开示例文件研究研究即可