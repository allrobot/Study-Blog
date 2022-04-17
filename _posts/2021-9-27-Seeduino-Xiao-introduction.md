---
layout: article
title:  "Seeeduino-XIAO入门"
date:   2021-09-27
categories: 硬件开发
tags: [Arduino,硬件开发,XIAO]
---

Seeeduino XIAO

## 配置：

- 32位单核处理器
- 时钟速度高达 48MHz
- 32KB SRAM
- 256KB 闪存
- 11 数字输入/输出
- 11 个 12 位 ADC 输入
- 1个DAC输出
- 支持多达 120 个触控通道
- USB 设备和嵌入式主机
- 硬件实时时钟
- 1.62V 至 3.63V 电源
- 极低的功耗
- 6 个串行通信模块 (SERCOM) 可配置为 UART/USART、SPI 或 I2C

## 引脚：

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   | 13   |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|A0|A1|A2|A3|A4|A5|A6|A7|A8|A9|A10|3V3|GND|5V|
|D0|D1|D2|D3|D4|D5|D6|D7|D8|D9|D10|输出端|GND|输入端|
|DAC|REF| | |SDA|SCL|TX|RX|SCK|MISO|MOSI| | ||
| INT_2 |INT_4|INT_10|INT_11|NMI|INT_9|INT_8|INT_9|INT_7|INT_5|INT_6| | | |

- DAC ——（引脚 0）——这是数模转换器的输出，它产生 0 至 3.3 伏的模拟输出。
- REF ——（引脚 1）——这是模拟输入使用的 11 个模数转换器的参考电压。它相当于 Arduino 上的 AREF 引脚，并且可以接受最大 3.3 伏的电压。
- SDA ——（引脚 4）——I2C 数据线。请注意与 Arduino Uno 类似的引脚排列，它也使用 A4 作为 SDA。
- SCL ——（引脚 5）——I2C 时钟线。这又是与 Uno 相同的引脚排列。
- TX ——（引脚 6）——UART 发送线。
- RX ——（引脚 7）——UART 接收线。
- SCK ——（引脚 8）——SPI 总线时钟线。
- MISO ——（引脚 9）——SPI 总线的主输入从输出。
- MOSI ——（引脚 10）——SPI 总线的主输出从输入。

>同时接线RST(Reset)，重置芯片数据
- 大多数中断都是“可屏蔽的”，换句话说，您可以通过编程方式选择忽略它们。
- 引脚 4 上的中断是不可屏蔽的，不能以编程方式忽略它。
- 引脚 5 和 7 共享相同的中断 (INT_9)。

灯L |灯T|灯R|灯P
--|--|--|--
User Led|UART TX|UART RX|Power
D13-YEL|D11-BLU|D12-BLU|GRN

- L – (D13) – 这是用户或内置 LED。它连接到数据 I/O 引脚 13，就像 Arduino Uno 上的内置 LED 一样。此 LED 为黄色。
- T – (D11) – 这是传输 LED，它指示 USB-C 总线上的传输活动。它也可以作为数据 I/O 引脚 11 寻址。该 LED 为蓝色。
- R – (D12) – 这是接收 LED，指示 USB-C 总线上的活动。它也可以作为数据 I/O 引脚 12 寻址。该 LED 也是蓝色的。
- P - 这是电源指示灯，LED 为绿色。


## 有几种不同的方式为 Seeeduino XIAO 供电：

USB-C 连接器通过连接的计算机为 XIAO 供电。
- 您可以在引脚 13 上施加 5 伏电压，内部 DC-DC 转换器会将其降低到 3.3 伏。
- 您可以在引脚 11 上施加 3.3 伏电压。
- 您可以使用 XIAO 下方的 VCC 和 GND 连接并施加 3.7 至 5 伏电压。这是使用小型锂聚合物电池为 XIAO 供电的理想方法。

电源引脚可用作输入或输出，因为它们连接在 DC-DC 转换器上。如果您通过 USB-C 连接为 XIAO 供电，那么您可以将电源引脚用作输出，在引脚 13 上提供 5 伏电压，在引脚 11 上提供 3.3 伏电压。

如果将 5 伏电压施加到引脚 13，则可以将引脚 11 用作 3.3 伏输出。您还可以通过提供自己的 3.3 伏电源并将其连接到引脚 11 来为 XIAO 供电——在这种情况下，5 伏引脚不起作用。

SWCLK和SWDIO是串行调试功能的时钟和数据线

- 支持Arduino
- 支持CircuitPython

## Arduino添加Seeeduino XIAO

1. Arduino IDE-->文件-->首选项-->附加开发板管理器-->添加"https://files.seeedstudio.com/arduino/package_seeeduino_boards_index.json"

2. Arduino IDE-->项目-->项目库-->管理库-->搜索"seeedstudio XIAO"-->安装

3. Arduino IDE-->工具-->开发板-->Seeeduino XIAO-->端口

## Arduino代码

Seeeduino XIAO 有 11 个模拟输入，每个都连接到一个 12 位 A/D 转换器。
>输入施加的最大电压为 3.3 伏

### 打印模拟输入值

```C
/*
  Seeeduino XIAO Analog Input Demo
  xiao-analog-in-test.ino
  Displays operation of XIAO A/D converter
  Results displayed on Serial Monitor
  显示XIAO A/D转换器的运行情况  
  结果显示在Serial Monitor上  
  DroneBot Workshop 2020
  https://dronebotworkshop.com
*/

// Analog Input Pin
//模拟输入端口
#define ANALOG_IN_PIN A2

// Integer to represent input value
//设输入值
int input_val;

void setup() 
{
  // Set A/D converter resolution to 12-bits
  //设置A/D分辨率12比特
  analogReadResolution(12); 
  
  // Setup Serial Port
  //初始化串口端口
  SerialUSB.begin(9600);
}

void loop() 
{
  // Read the input value
  //读取A2的输入值
  input_val = analogRead(ANALOG_IN_PIN);  
  
  
  // Print value to Serial Monitor
  //把输入值打印到端口上
  SerialUSB.println(input_val);
  
  // Slight delay before repeating
  延迟10毫秒，1000毫秒=1秒
  delay(10); 
  
}
```

其中的loop可以是：
```C
   // 读取模拟引脚A0的输入数据
   int sensorValue = analogRead(A0);
   // 将模拟信号转换成电压
   float voltage = sensorValue * (5.0 / 1023.0);
   // 打印到串口监视器
   Serial.println(voltage);
```

### 打印数字I/O和内置LED

引脚9接二极管灯管的VIN引脚，灯管另一个引脚接地；
引脚7接四角按钮的左上一脚，左下的一脚接地
```C
/*
  Seeeduino XIAO Digital I/O Demo
  xiao-digital-io-test.ino
  Displays operation of XIAO digital inputs and outputs
  Results displayed on serial monitor
  显示XIAO数字输入输出的运行情况
  结果显示在串行监视器上
  DroneBot Workshop 2020
  https://dronebotworkshop.com
*/

// Pushbutton input
//按钮7，按下引发引脚7电压变化
#define BUTTON 7

// LED output
//引脚9，灯管电源于此引脚
#define LED 9

// Variable to hold state of pushbutton
boolean buttonState;

void setup()
{
    // Setup the pushbutton as an input and enable internal pullup
	//设按引脚7为输入端，启动内部上拉
    pinMode(BUTTON, INPUT_PULLUP);
    
    // Setup the LED as an output
	//设引脚9作为输出端
    pinMode(LED, OUTPUT);
    
    // Also define the builtin LED as an output
	//也设内置灯LED L作为输出端
    pinMode(LED_BUILTIN, OUTPUT);
    
    // Setup Serial Port
    SerialUSB.begin(9600);

}

void loop()
{
    // Get the pushbutton state and assign it to the variable
	//逻辑判断，数字读取引脚7按钮状态
    buttonState = digitalRead(BUTTON);
    
    // Print pushbutton state to serial monitor
	//打印该状态
    SerialUSB.println(buttonState);
    
    // Set the LED according to pushbutton state
	//写入逻辑值输出到外接的灯管
    digitalWrite(LED, buttonState);
    
     // Set the builtin LED according to pushbutton state
	 //也写入到内置LED L
    digitalWrite(LED_BUILTIN, buttonState);
    
}
```

### 模拟输出

```C
/*
  Seeeduino XIAO Analog Output Demo
  xiao-analog-out-test.ino
  Displays operation of XIAO D/A converter
  Produces a sine wave on Analog output
  Reads result on analog input
  Results displayed on Serial Plotter
  显示XIAO D/A转换器的运行情况
  模拟输出产生正弦波
  读取模拟输入的结果
  结果显示在Serial Plotter上
  Adapted from Seeeduino sketch on https://wiki.seeedstudio.com/Seeeduino-XIAO

  DroneBot Workshop 2020
  https://dronebotworkshop.com
*/

// Define Analog Out pin
#define DAC_PIN A0

// Define Analog In pin
#define ADC_PIN A4

// Float output to modify for sine wave
//输出
float outval = 0;
// Increment
//变量
float increment = 0.02;


void setup()
{
  // Set analog output resolution to 10-bits
  //模拟输出分辨率10比特
  analogWriteResolution(10);

  // Set analog input resolution to 12-bits
  //模拟输入分辨率12比特
  analogReadResolution(12);

  // Setup Serial Port
  SerialUSB.begin(9600);
}

void loop()
{
  // Generate a voltage value between 0 and 1023.
  //生成0到1023之间的电压值。
  // Offset by 511.5 (half-point)
  int dacVoltage = (int)(511.5 + 511.5 * sin(outval));

  // Increment
  outval += increment;

  // Output to DAC port
  //数字写入转为模拟输出
  analogWrite(DAC_PIN, dacVoltage);

  // Read DAC output on ADC input
  //在ADC端口读取DAC输出值
  float voltage = analogRead(ADC_PIN) * 3.3 / 4096.0;

  // Print to serial plotter
  //打印DAC输出值
  SerialUSB.println(voltage);

  // 1ms delay (change to change sine wave frequency改变正弦波频率)
  delay(1);
}
```