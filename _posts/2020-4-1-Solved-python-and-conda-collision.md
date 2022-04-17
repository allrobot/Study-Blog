---
layout: article
title:  "解决python与conda的环境变量冲突"
date:   2020-4-1
categories: python
tags: [python,conda]
---

复制指定python.exe更名为xxx.exe

cmd命令:

- 输入`xxxxx –m pip –-version` 查看当前Python对应的pip版本；
	
- 输入`xxxxx –m pip list`      查看当前Python对应的pip安装的第三方库；
	
- 输入`xxxxx –m pip install`   库名即可安装对应的扩展库；
	
- 输入`xxxxx –m pip uninstall` 库名即可卸载对应的扩展库