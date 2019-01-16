---
title: python-debug
date: 2018-01-15 16:29:58
tags: Python
---
* pdb debug
pdb是python自带的库，为python程序提供了交互式的代码调试库。
明显的缺陷就是对于多线程，远程调试等支持得不够好，同时没有较为直观的界面显示，不太适合大型的 python 项目。而在较大的 python 项目中，这些调试需求比较常见，因此需要使用更为高级的调试工具。


c继续执行程序
l查看当前行的代码
s进入函数
q中止并退出
n执行下一行
p打印变量的值
```
python -m pdb test.py
```

调用pdb模块的set_trace方法设置一个断点，当程序运行自此时，将会暂停执行并打开pdb调试器。
```
import pdb
a = "a string"
pdb.set_trace()
(pdb) p a // 打印出a
(pdb)
```
* pudb

```
pip install pudb // 安装pudb
```
