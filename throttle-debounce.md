---
title: throttle-debounce
date: 2018-06-25 16:42:27
tags:
---
* throttle(节流): 设置一个阈值，在阈值内，把触发的事件合并成一次执行，当达到阈值，必行执行
* debounce(去抖动): 把触发非常频繁的事件合并成一次执行 

## throttle 
###代码
``` javascript
const throttle = (fn, delay) => {
  const startTime;
  return () {
    const curTime = new Date();
    if (curTime - startTime>= delay) {
      fn.apply(this, arguments);
      startTime = curTime;
    }
  }
};
```
### 适用场景
频繁触发时间，且在一定时间内需要执行
1. 鼠标移动，
2. 窗口滚动
## debounce 
###代码
```javascript
const debounce = () => {
 let timer = null;
  return function() {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, arguments);
    }, delay);
  }
};
```

### 适用场景
频繁触发事件, 且可以合并处理
1. 用户输入搜索
2. 浏览器resize
