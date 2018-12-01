---
title: react 入门
date: 2017-06-16 11:33:27
tags: React
---
### 遇到的问题
* 报语法错误，最后排除发现，component里li元素的标签没有闭合
```

performUpdateIfNecessary: Unexpected batch number
//没有定位到具体出错的代码
原因是新增加的组件里没有对应的reducer
```
