---
layout: article
title:  "BAT查询Android studio的虚拟机列表"
date:   2021-11-1
categories: Android
tags: [Android,emulator]
---

```BASH
::CMD查询模拟器列表

for /f %%i in ('emulator -list-avds') do 
(
emulator %%i 
)
```



