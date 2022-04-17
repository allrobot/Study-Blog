---
layout: article
title:  "XIAO的串口及检测采样频率"
date:   2021-09-27
categories: 硬件开发
tags: [XIAO,硬件开发,UART]
---

## 串口读取字符串
```C
void setup(){
//若干代码
}
void loop()
{
	String inString="";
	while(Serial.available()>0)
	{
		char inChar=Serial.read();
		inString+=(char)inChar;
		delay(10);
	}
	if(inString!="")
	{
		SerialUSB.print("Input String: ");
		SerialUSB.println(inString);
	}
}
```
## 串口事件

```C
String inputString = "";         // a String to hold incoming data
bool stringComplete = false;  // whether the string is complete

void setup() {
  // initialize serial:
  SerialUSB.begin(9600);
  // reserve 200 bytes for the inputString:
  inputString.reserve(200);
}

void loop() {
  serialEvent1()
  // print the string when a newline arrives:
  if (stringComplete) {
    SerialUSB.println(inputString);
    // clear the string:
    inputString = "";
    stringComplete = false;
  }
}

/*
  SerialEvent occurs whenever a new data comes in the hardware serial RX. This
  routine is run between each time loop() runs, so using delay inside loop can
  delay response. Multiple bytes of data may be available.
*/
void serialEvent1() {
  while (Serial.available()) {
    // get the new byte:
    char inChar = (char)Serial.read();
    // add it to the inputString:
    inputString += inChar;
    // if the incoming character is a newline, set a flag so the main loop can
    // do something about it:
    if (inChar == '\n') {
      stringComplete = true;
	  break;
    }
  }
}
```

## 采样速度

8个传感器，16分辨率比特

```C
// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  analogReadResolution(16);
  SerialUSB.begin(9600);
}


void loop() {
  int i;
  float voltage;
  int sensorValue;
  unsigned long elsp=millis();
  for (i=0;i<10000;i++)
    {
      // read the input on analog pin 0:
      sensorValue = analogRead(A0);
      sensorValue = analogRead(A1);
      sensorValue = analogRead(A2);
      sensorValue = analogRead(A3);
      sensorValue = analogRead(A4);
      sensorValue = analogRead(A5);
      sensorValue = analogRead(A6);
      sensorValue = analogRead(A7);
    }  
  Serial.println(millis()-elsp);
  Serial.println("waiting...");
  delay(10000);
}
```
输出1150ms，既10000次1.15秒，采样频率=10000/1.15=8695次

