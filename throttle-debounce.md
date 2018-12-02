---
title: throttle-debounce
date: 2018-06-25 16:42:27
tags:
---
* throttle(节流): 设置一个阈值，在阈值内，把触发的事件合并成一次执行，当达到阈值，必行执行
* debounce(去抖动): 把触发非常频繁的事件合并成一次执行 

## throttle

### 方法一：时间戳
利用本次执行的时间与上次执行的时间相比较
特点：先执行目标函数，后等待规定的时间段, 一般首次都会触发, 最后一次如果时间不满，不会触发
``` javascript
function throttle(fn, delay) {
  let startTime = +new Date;
  return function() {
    const curTime = +new Date();
    if (curTime - startTime >= delay) {
      fn.apply(this.arguments);
      startTime = curTime;
    }
  };
}
```

### 方法二:定时器
利用定时器实现时间间隔
特点：先等待够规定时间，再执行，即停止触发后，若定时器已经在任务队列里注册了目标函数，它也会执行最后一次。首次不会触发

``` javascript
function throttle(fn, wait) {
  var time, context;
  return function() {
    context = this;
    if(!time) {
      time = setTimeout(function() {
        fn.apply(context, arguments);
        time = null;
      }, wait);
    }

  }
}
```

### 方法三：时间戳+定时器
```
function throttle(func, wait) {
  let previous = 0;
  let context, args, time, remaining;
  return function() {
    let now = +new Date();
    context = this;
    args = arguments;
    remaining = wait - (now - previous);
    if (remaining <= 0) {
      func.apply(context, args);
      previous = now;
    } else {
      if (time) clearTimeout(time);
      time = setTimeout(function() {
        func.apply(context, args);
        time = null;
        previous = +new Date();
      }, remaining);
    }
  }
};

```

### 方法四：underscore的实现方法
通过leading， trailing对首尾是否调用
leading：false 表示禁用第一次执行
trailing: false 表示禁用停止触发的回调

```
const throttle = function(func, wait, options) {
  var timeout, context, args, result;
  var previous = 0;
  if (!options) options = {};

  var later = function() {
    previous = options.leading === false ? 0 : +new Date;
    timeout = null;
    result = func.apply(context, args);
    if (!timeout) context = args = null;
  };

  var throttled = function() {
    var now = +new Date();
    if (!previous && options.leading === false) previous = now;
    var remaining = wait - (now - previous);
    context = this;
    args = arguments;
    if (remaining <= 0 || remaining > wait) {
      if (timeout) {
        clearTimeout(timeout);
        timeout = null;
      }
      previous = now;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    } else if (!timeout && options.trailing !== false) {
      timeout = setTimeout(later, remaining);
    }
    return result;
  };

  throttled.cancel = function() {
    clearTimeout(timeout);
    previous = 0;
    timeout = context = args = null;
  };

  return throttled;
};
```

### 适用场景
频繁触发时间，且在一定时间内需要执行
1. 鼠标移动，
2. 窗口滚动


## debounce 
###代码
```javascript
function debounce(func, wait) {
  var timeout;
  return function() {
    let context = this, args = arguments;
    clearTimeout(timeout);
    timeout = setTimeout(function() {
      func.apply(context, args);
    }, wait);

  };
}
```
有时候希望能实现 在一个周期内第一次触发，就立即执行一次，然后一定时间段内都不能再执行目标函数
优化后：
```
var debounce = function(func, wait, immediate) {
  var timeout, args, context, timestamp, result;
  var later = function() {
    var last = +new Date() - timestamp;
    if (last < wait && last >= 0) {
      timeout = setTimeout(later, wait - last);
    } else {
      timeout = null;
      if (!immediate) {
        result = func.apply(context, args);
        if (!timeout) context = args = null;
      }
    }
  };

  return function() {
    context = this;
    args = arguments;
    timestamp = +new Date();
    var callNow = immediate && !timeout;
    if (!timeout) timeout = setTimeout(later, wait);
    if (callNow) {
      result = func.apply(context, args);
      context = args = null;
    }

    return result;
  };
};
```
如果immediate为true的话，立刻调用，然后wait内不可以再次调用，
可以用于提交等操作

### 适用场景
频繁触发事件, 且可以合并处理
1. 用户输入搜索
2. 浏览器resize
