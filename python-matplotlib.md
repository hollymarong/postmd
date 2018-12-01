---
title: python-matplotlib
date: 2018-01-16 09:41:20
tags: python
---
* Matplotlib
是一个非常强大的Python画图工具
```
pip install matplotlib

```
使用时matplotlib是，macos会提示错误Python is not installed as a framework
解决方法：
```
在~/.matploglib/ 下新建文件matploglibrc
// matplotlibrc
backend: TkAgg
```
