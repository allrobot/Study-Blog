---
layout: article
title:  "Win10安装PyBluez"
date:   2022-03-17
categories: python
tags: [python,BT,Pybluez]
---

## win10导入PyBluez库
Windows10安装python第三方库pybluez步骤。这些步骤假设基于 conda 的 Python 安装，例如Anaconda。

### 第一步：下载最新代码
在GIT Bash拉取库代码
```
C:\Users\me> git clone https://github.com/pybluez/pybluez
C:\Users\me> cd pybluez
```

### 第 2 步：创建环境

新建环境Python==3.6版本使之构建 pybluez。步骤适用于其他版本，例如 Python 2.7等。
```
C:\Users\me\pybluez> conda create -n pybluez python==3.6
C:\Users\me\pybluez> activate pybluez
```

python -m pip install --upgrade pip

### 第 3 步：构建库

请注意，在运行构建之前，必须为所需的 Python 版本安装正确的 C++ 编译器。可以在 Python wiki WindowsCompilers 页面上找到编译器的下载链接。
```
(pybluez) C:\Users\me\pybluez> python setup.py install
(pybluez) C:\Users\me\pybluez> python setup.py bdist_wheel
(pybluez) C:\Users\me\pybluez> deactivate
```
这两个步骤将在文件夹中创建一个.whl文件dist。pybluez 库现在可以通过将这个 wheel 文件 pip 安装到项目的 virtualenv 中来在另一个项目中使用。例如：

C:\Users\me\pybluez> cd ..\project
C:\Users\me\project> conda create -n project python==3.6
C:\Users\me\project> activate project
(project) C:\Users\me\project> pip install ..\pybluez\dist\PyBluez-0.22-cp36-cp36m-win_amd64.whl
(project) C:\Users\me\project> python -c "import bluetooth; print(bluetooth.__version__)"

我进入`tensorflow`虚拟环境，结果报错如下：
```
(tensorflow) C:\Users\Administrator\pybluez>python setup.py install
...
error: command 'cl.exe' failed: No such file or directory
```
[解决方案](https://stackoverflow.com/questions/48821917/downloading-and-installing-pybluez-for-a-64-bit-windows-10-pc/58980599#58980599)




#### 设置cl.exe 环境变量（vs2017）

在环境变量中Path 那一项中添加两个路径（记得加分号）：
```
E:\VS2017\VC\Tools\MSVC\14.10.25017\bin\HostX64\x64;
E:\VS2017\Common7\IDE
```
第一行是编译器的目录，根据电脑选吧，但是至少HostX86\x86是所有平台都能用，64位系统就像例子中那么填
在环境变量中新建一个LIB 变量，并添加三个路径（记得加分号）：
```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.10.25017\lib\x64;
C:\Program Files (x86)\Windows Kits\10\Lib\10.0.14393.0\ucrt\x86;
C:\Program Files (x86)\Windows Kits\8.1\Lib\winv6.3\um\x86;
```
第一个路径填的是lib路径，如果在path变量中填了HostX64\x64的话就用x64的lib
第二个路径填的我也不知道，但是填就填最新版的话，目前没发现什么问题

在环境变量中新建一个INCLUDE 变量，并添加两个路径（记得加分号）：
```
E:\VS2017\VC\Tools\MSVC\14.10.25017\include;
C:\Program Files (x86)\Windows Kits\10\Include\10.0.14393.0\ucrt;
```
第二个路径和上面第二个路径对应

在E:\VS2017\VC\Auxiliary\Build 这个路径中点击vcvarall.bat 这个批处理程序。之后打开命令行，输入cl，如果没有报错，则配置成功……

安装的时候要选定`WINDOWS SDK 10`
实际路径根据自己的安装路径有所不同。

>若该办法没效果，看看注册表HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor有没有autorun字符串值`chcp 65001`，它虽然使控制台为utf-8编码，但会破坏很多程序利用cmd完成一些事情，比如

### 第四步 客户端接收通知

蓝牙存在连接间隔，每秒只能发送20~30个数据包，python客户端存在丢包现象

bleak Code:
```PYTHON
# Service UUID: 6E400001-B5A3-F393-E0A9-E50E24DCCA9E
# TX UUID:      ca73b3ba-39f6-4ab3-91ae-186dc9577d99
# MAC:          7C:9E:BD:48:6A:36
# Name:         UART Service
# 每16次采样就采集一次
import sys
import time
import datetime
import asyncio
import logging

from bleak import BleakClient



logger = logging.getLogger()


# 运行BLE客户端
async def run_ble_client(address: str, char_uuid: str, queue: asyncio.Queue):
    # 将一个目标数据放入队列中。如果队列已满，则在添加项之前等待，直到有空队列可用
    async def callback_handler(sender, data):
        await queue.put((time.time(), data))

    # BLE通常的连接间隔约为30到50毫秒，因此每秒只能发送20到30个数据包。
    # HID设备可以有15毫秒的最快间隔（取决于配置），即每秒66个数据包。
    # sEMG每秒发送36000字节，需调整
    # 大多数设备的最大MTU为512字节，因此这将产生66 * 512 = 大约34000字节每秒
    # 因此，ESP32上的缓冲区可能因试图以比可能更快的速度发送数据而溢出。
    # 创建一个客户端事件，并且定时决定什么时候中断接收
    async with BleakClient(address) as client:
        # 打印BLE连接状态
        logger.info(f"Connected: {client.is_connected}")
        # 开启客户端通知
        await client.start_notify(char_uuid, callback_handler)
        while True:
            await asyncio.sleep(3600.0)
        # await client.stop_notify(char_uuid)
        # Send an "exit command to the consumer"退出命令到外部设备
        await queue.put((time.time(), None))

# 打印外部设备传来的信息队列
async def run_queue_consumer(queue: asyncio.Queue):
    while True:
        # Use await asyncio.wait_for(queue.get(), timeout=1.0) if you want a timeout for getting data.
        # 如果你需要一个获取数据的超时时间,在asyncio.wait_for(queue.get(), timeout=1.0)设置timeout值
        epoch, data = await queue.get()
        # 如果未收到信息,输出
        if data is None:
            logger.info(
                "Got message from client about disconnection. Exiting consumer loop..."
            )
            break
        else:
            print(data)


async def main(address: str, char_uuid: str):
    # 设置队列，用于协调生产者和消费者协程。
    queue = asyncio.Queue()
    # 创建客户端任务
    client_task = run_ble_client(address, char_uuid, queue)
    # 运行打印循环
    consumer_task = run_queue_consumer(queue)
    #
    await asyncio.gather(client_task, consumer_task)
    logger.info("Main method done.")


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    asyncio.run(
        main(
            "7C:9E:BD:48:6A:36",
            f"ca73b3ba-39f6-4ab3-91ae-186dc9577d99",
        )
    )

```

### python 显示图标
```
from PyQt5 import QtCore, QtGui, QtWidgets
from pyqtgraph import PlotWidget
import pyqtgraph as pg
import numpy as np
```

```PYTHON
        Form.resize(820, 454)
        Form.setObjectName("Form")
        historyLength=1800
        self.graphicsView = PlotWidget(Form)
        self.graphicsView.setGeometry(QtCore.QRect(10, 10, 261, 121))
        self.graphicsView.setObjectName("graphicsView")

        self.graphicsView.setLabel(axis="left",text='sEMG Voltage')
        self.graphicsView.setLabel(axis='bottom',text='Time')
        self.graphicsView.setMouseEnabled(x=False,y=False)
        self.graphicsView.setAntialiasing(True)

        self.data1 = np.random.normal(size=1700)
        self.curve2 = self.graphicsView.plot(self.data1)
        self.ptr1 = 0

        def update1():
            global data1, ptr1
            self.graphicsView.setRange(xRange=[self.ptr1,self.ptr1+historyLength],yRange=[0,500],padding=0)
            self.data1[:-1] = self.data1[1:]  # shift data in the array one sample left

            self.data1[-1] = np.random.normal(300)

            self.ptr1 += 1
            self.curve2.setData(self.data1)
            self.curve2.setPos(self.ptr1, 0)

        self.timer = pg.QtCore.QTimer()
        self.timer.timeout.connect(update1)
        self.timer.start(10)
```