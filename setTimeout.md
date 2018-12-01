---
title: 定时器
date: 2017-03-24 19:55:59
tags: JavaScript
---
## 定时器
由于JavaScript是单线程的特性，Javascript是同一时间只能执行一段代码，异步事件的处理程序，需要在线程中没有代码执行的时候才能执行,定时器的延迟时间并不按照指定的时间间隔触发。
```javascript 
var count = 0;
var start = +new Date();
for(var n = 0; n < 1000; n++){
    console.log('n');
}
console.log('n is over time ', +new Date - start);
setTimeout(function(){
    console.log('setTimeout1', 'time ', (+new Date - start));
},10);
var interval = setInterval(function(){
    count++
    console.log('setInterval ',count, ' time ', (+new Date - start));
    if(count == 100){
        clearInterval(interval);
    }
}, 10);
setTimeout(function(){
    console.log('setTimeout2', 'time ', (+new Date - start));
},10);

console.log('start');
for(var m = 0; m< 1000; m++){
    console.log('m');
}
console.log('m is over time', +new Date - start);
console.log('end');
//执行结果
n
 n is over time  28
start
m
m is over time 50
end
setTimeout1 time  51
setInterval  1  time  52
setTimeout2 time  52
setInterval  2  time  60
setInterval  3  time  70
setInterval  4  time  80
setInterval  5  time  90
setInterval  6  time  100
setInterval  7  time  110
setInterval  8  time  120
setInterval  9  time  130
setInterval  10  time  140
setInterval  11  time  150
```
![线程代码执行](/css/images/timer.png)
由此可以看出,异步事件发生时，就会排队，只在线程空闲的时候才执行。
开始启动一个10毫秒的延迟的定时器，一个10毫秒的间隔定时器
主线代码执行了50毫秒,在10毫秒的时候，setTimeout1,setInterval, setTimeout2应该立即执行，但是线程里还在执行主线代码，setTimeout1，setInterval, setTimeout2排队等待线程空闲。50毫秒后，主线代码执行结束，开始执行setTimeout1,setInterval,setTimeout2。
浏览器只对一个setInterval的实例进行排队， 没有对多个实例进行排队，后续线程中没有排队的处理程序，setInterval每隔10毫秒执行。

