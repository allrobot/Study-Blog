---
layout: article
title:  "失败的ESP32缓存UART数值文件"
date:   2022-03-20
categories: 硬件开发
tags: [硬件开发,ESP32]
---

#include <BLEDevice.h>
#include <BLEServer.h>
#include <BLEUtils.h>
#include <BLE2902.h>
// #include <periph_ctr
#define SERIAL_RX_BUFFER_SIZE 1024
#define SERIAL_TX_BUFFER_SIZE 1024
BLEServer *pServer = NULL;
BLECharacteristic *pTxCharacteristic;
bool deviceConnected = false;
bool oldDeviceConnected = false;

uint8_t txValue = 0;

bool Start = false;
bool SendStart = false;

// See the following for generating UUIDs:
// https://www.uuidgenerator.net/

#define SERVICE_UUID "6E400001-B5A3-F393-E0A9-E50E24DCCA9E" // UART service UUID
#define CHARACTERISTIC_UUID_RX "f78ebbff-c8b7-4107-93de-889a6a06d408"
#define CHARACTERISTIC_UUID_TX "ca73b3ba-39f6-4ab3-91ae-186dc9577d99"

class MyServerCallbacks : public BLEServerCallbacks
{
  void onConnect(BLEServer *pServer)
  {
    deviceConnected = true;
  };

  void onDisconnect(BLEServer *pServer)
  {
    deviceConnected = false;
  }
};

class MyCallbacks : public BLECharacteristicCallbacks
{
  void onWrite(BLECharacteristic *pCharacteristic)
  {
    std::string rxValue = pCharacteristic->getValue();

    if (rxValue.length() > 0)
    {
      Serial2.println("*********");
      Serial2.print("Received Value: ");
      for (int i = 0; i < rxValue.length(); i++)
        Serial2.print(rxValue[i]);

      Serial2.println();
      Serial2.println("*********");
    }
  }
};
void setup()
{
  Serial1.begin(500000, SERIAL_8N1, 3, 1);
  Serial2.begin(500000, SERIAL_8N1, 16, 17);

  // Create the BLE Device
  BLEDevice::init("UART Service");

  // Create the BLE Server
  pServer = BLEDevice::createServer();
  pServer->setCallbacks(new MyServerCallbacks());

  // Create the BLE Service
  BLEService *pService = pServer->createService(SERVICE_UUID);

  // Create a BLE Characteristic
  pTxCharacteristic = pService->createCharacteristic(
      CHARACTERISTIC_UUID_TX,
      BLECharacteristic::PROPERTY_NOTIFY);

  pTxCharacteristic->addDescriptor(new BLE2902());

  BLECharacteristic *pRxCharacteristic = pService->createCharacteristic(
      CHARACTERISTIC_UUID_RX,
      BLECharacteristic::PROPERTY_WRITE);

  pRxCharacteristic->setCallbacks(new MyCallbacks());

  // Start the service
  pService->start();

  // Start advertising
  pServer->getAdvertising()->start();

  // // 确定传感器的连接状态
  // // 缓冲区没有字符时，每半秒发送1次
  // while (true)
  // {
  //   if (Serial1.available() <= 0)
  //   {
  //     Serial1.print('A');
  //     delay(500);
  //   }
  //   else
  //   {
  //     while (Serial1.available()>=0)
  //     {
  //       Serial1.read();
  //     }
  //     break;
  //   }
  // }
  Serial2.println("\nSet Serial1 ok!");
  Serial2.println("Waiting a client connection to notify...");
}

// void loop()
// {
//   recvWithEndMarker();
//   showNewData();
// }
// 接收串口数据，并

void recvWithEndMarker()
{
  if (Start)
  {
    const byte numChars = 144;
    char receivedChars[144];
    char MTU[288];
    static bool newData = false;
    static int merge_string_coordinates = 0;
    static int merge_index = 0;
    for (int I = 0; I < 3; I++)
    {
      static byte ndx = 0;
      static bool recvInProgress = false;
      char startMaker = '<';
      char endMarker = '>';
      char rc;
      // if (Serial1.available() > 0) {
      while (Serial1.available() > 0 && newData == false)
      {
        rc = Serial1.read();
        if (recvInProgress == true)
        {
          if (rc != endMarker)
          {
            receivedChars[ndx] = rc;
            ndx++;
            if (ndx >= numChars)
            {
              ndx = numChars - 1;
            }
          }
          else
          {
            receivedChars[ndx] = '\0'; // terminate the string
            recvInProgress = false;
            ndx = 0;
            newData = true;
          }
        }
        else if (rc == startMaker)
        {
          recvInProgress = true;
        }
      }
      if (newData)
      {
        Serial2.print(merge_string_coordinates);
        Serial2.print(": ");
        Serial2.println(receivedChars);
        static int receivedChars_length = strlen(receivedChars);

        static int MTUlength = strlen(MTU);
        if (merge_string_coordinates == 0 && newData == true)
        {
          for (int loop = 0; loop < receivedChars_length; loop++)
          {
            MTU[merge_index] = receivedChars[loop];
            merge_index++;
          }
          MTU[merge_index] = ':';
          merge_index += 1;
          MTU[merge_index] = '\0';
          merge_string_coordinates = 1;
          newData = false;
        }
        else if (merge_string_coordinates == 1 && newData == true)
        {
          for (int loop = 0; loop < receivedChars_length; loop++)
          {
            MTU[merge_index] = receivedChars[loop];
            merge_index++;
          }
          MTU[merge_index] = ':';
          merge_index += 1;
          MTU[merge_index] = '\0';
          newData = false;
          merge_string_coordinates = 2;
        }
        else if (merge_string_coordinates == 2 && newData == true)
        {
          for (int loop = 0; loop < receivedChars_length; loop++)
          {
            MTU[merge_index] = receivedChars[loop];
            merge_index++;
          }
          MTU[merge_index] = '\0';
          SendStart = true;
          merge_string_coordinates = 0;
          merge_index = 0;
          Serial2.println(MTU);
          pTxCharacteristic->setValue(MTU);
          pTxCharacteristic->notify();
        }
      }
    }
  }
}
// 输出数据
// void showNewData(char MTU)
// {
//   // if (newData == true)
//   // {

//   // newData = false;
//   // }
// }
void loop()
{
  // disconnecting
  if (!deviceConnected && oldDeviceConnected)
  {
    Start = false;
    delay(500);                  // 让蓝牙堆栈有机会做好准备
    pServer->startAdvertising(); // 重启广播
    Serial2.println("disconnecting start advertising");
    oldDeviceConnected = deviceConnected;
    ESP.restart();
  }
  // connecting
  if (deviceConnected && !oldDeviceConnected)
  {
    // do stuff here on connecting在连接上做点事情
    oldDeviceConnected = deviceConnected;
    Serial2.println("connecting start advertising");
  }
  if (deviceConnected)
  {
    Start = true;
    recvWithEndMarker();
    // if (SendStart)
    // {
    //   showNewData();
    // }
  }
}