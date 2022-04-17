---
layout: article
title:  "EMG硬件相关的调试经验"
date:   2022-03-18
categories: 硬件开发
tags: [机器学习,sEMG,数据处理,硬件]
---

## UART串口
### 串口引脚
1. 先搞懂微控制器和蓝牙的引脚，然后arduino库调用板子的引脚进行串口传输。

### 串口配置
1. 串口传输需要注意发送方和接收方波特率一致

2. 在arduino中115200波特率和SERIAL_8N1
	- 115200波特率，115200位/秒
	- SERIAL_8N1，10位/字符，包含校验、停止位，每秒发送11520个字符
	
### 串口传输格式

1. 发送更高效的数值格式，传输效率至少提高3倍；
(比如二进制数值)，而不是发送格式化的ASCII数据

2. 敏感数据应编写握手协议，虽然会使传输效率降低。
协议大概内容：接收方确认后发送OK，发送方收到OK才能发送下一个数据。

3. **暂不清楚格式数据如何编写，使提高传输效率**

## 目标输出6通道数值

### 测量UART输出上限

输出格式"xx:xx:xx...xx:xx"

1. 微控制器Serial.printf("%d:%d...%d",x,x...x)目前已在seeeduino xiao测试过，ERP32 DEVKIT V1只能接收5\*6+5+2=37个字符串，5\*8+7+2=49字符串无法输出
>后面的2是'\n'

即使for循环输出Serial.printf("%d",x)也不行

**不晓得什么原因，暂无解决方案**

### 测量

1. 确保频率200HZ以上，使用库文件示例代码，编写判断采样频率的小程序输出采样频率

2. 可以在void setup()写个while(!Serial)等待接通串口

3. 根据采样频率决定对硬件、代码进行处理


## 传感器输出干扰
干电极肌电传感器集成了滤波、放大电路，将范围在±1.5mV内的微弱人体表面肌电信号进行1000倍放大，并通过差分输入、模拟滤波电路的方式对噪音（特别是工频干扰）进行有效抑制。
  
输出信号为模拟量形式，以1.5V为基准电压，0～3.0V量程的输出。输出信号的大小取决于选定肌肉的活动量，输出信号的波形可显著指示被观察位置皮下肌肉的情况，方便做肌电信号的分析与研究，如使用Arduino作为控制器检测肌肉活动情况，如肌肉是否紧绷，强度如何，是否疲劳等。

干扰源：
- 静电感应

- 电磁感应

- 漏电流感应

- 射频干扰

- 其他干扰

笔记本USB给seeeduino xiao供电

1. 笔记本插电源适配器，USB给微控制器供电，微控制器3.3V引脚给传感器供电

信号输出不稳定

>有时候稳定10~50，不知道什么原因

分析：
昨天场景：插笔记本电源适配器，UART外接USB，直接USB-type-c供电，传感器直插xiao左侧GND-3.3V-A10引脚，信号输出0~400范围，很少超过400

今天重现场景，信号200000范围波动，**原因未知**。。。

不确定干扰源是哪些。。。

2. TTL转USB自带供电引脚，给微控制器供电，信号输出稳定输出30~65范围波动，未插传感器

usb线插传感器，烧录后再断开，保持TTL转USB的供电，此时Y轴突然变几万

干扰源可能来源于此？


  int data = analogRead(SensorInputPin);
  int dataAfterFilter = myFilter.update(data);  
  int envelope = sq(dataAfterFilter);   
  envelope = (envelope > threshold) ? envelope : 0;    
  Serial1.println(envelope);
  if (threshold > 0) 
  {
    if (getEMGCount(envelope)) 
    {  EMG_num++;
      Serial1.print("EMG_num: ");
      Serial1.println(EMG_num);
    }
  }
  delayMicroseconds(500);
  
    unsigned long elsp = millis();
  for (int i = 0; i < 1000; i++)
  {
    for (int i = 0; i < 6; i++) {

        int data            = analogRead(SensorInputPin[i]);
        int dataAfterFilter = myFilter.update(data);
        int envelope        = sq(dataAfterFilter);
        envelope            = (envelope > thresholds[i]) ? envelope : 0;
        Serial1.println(envelope);

        delayMicroseconds(500);
    }
  }
    Serial1.printf("XIAO Time: %d \n", millis() - elsp);
  Serial1.println(Serial1.availableForWrite());