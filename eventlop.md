---
title: eventlop
date: 2018-08-01 21:57:05
tags: javascript
---

* JS是单线程，虽然有web-worker多线程出现，但也是在主线程的控制下，web-worker仅仅能计算任务，不能操作dom，所以本质上还是单线程.
单线程的任务是串行的，后一个任务需要等待前一个任务的执行，这就可能出现长时间的等待，但由于类似ajax网络请求，setTimeout时间延迟、DOM事件的用户交互等，这些任务并不消耗CPU，是一种空等，浪费资源，因此出现了异步。通过将任务交给相应的异步模块去处理，主线程的效率大大提升，可以并行的去处理其它的操作。当异步处理完成，主线程空闲时，主线程读取响应的callback，进行后续的操作，最大程度的利用CPU。此时出现了同步和异步执行的概念,同步执行是主线程按照顺序，串行执行任务；异步执行就是cpu跳过等待，先处理后续的任务（CPU, 与网络模块、timer等并行进行任务）.因此产生了任务队列与事件循环，来协调主线程与异步模块之前的工作。
* 不同任务队列之间，存在着优先级，优先级高的先获取。
任务队列有两种类型，一种是microtask queue, 另一种是macrotask queue。
microtask queue:
  唯一，整个事件循环中，仅存在一个；执行为同步，同一个事件循环中的microtask会按队列顺序，串行执行完毕；process.nextTick, Promise, Object.observer, MutationObserver
macrotask queue:
  不唯一，存在一定的优先级（用户I/O部分优先级更高）;异步执行,同一个事件循环中，只执行一个。setTimeout setInterval,setImmediate, I/O, UI rendering

* 事件循环流程：
主线程读取JS代码，此时为同步环境，形成相应的堆和执行栈
主线程遇到异步任务，制定给对应的异步进程进行处理
异步进程处理完毕（ajax返回， DOM事件，timer）, 将相应的异步任务推入任务队列
主线程查询任务队列， 执行microtask(微任务) queue, 将其按顺序执行，全部执行完毕
主线程查询任务队列，执行macrotask(宏任务) queue， 取队首任务执行，执行完毕。
重复查询任务队列
microtask queue中的所有callback处在同一个事件循环中，而macrotask queue中的callback有自己的事件循环
同步环境执行 -> 事件循环1（microtask queue all） -> 事件循环2(macrotask queue中的一个) -> 事件循环1（microtask queue all）
-> 事件循环2(macrotask queue中的一个)

```
  console.log('1, time = ' + new Date().toString())
  setTimeout(macroCallback, 0);
  new Promise(function(resolve, reject) {
      console.log('2, time = ' + new Date().toString())
      resolve();
      console.log('3, time = ' + new Date().toString())
  }).then(microCallback);

  function macroCallback() {
      console.log('4, time = ' + new Date().toString())
  }

  function microCallback() {
      console.log('5, time = ' + new Date().toString())
  }
// 执行结果：
同步执行：1，2，3
事件循环1(micro callback): 5
事件循环2(macro callback): 4

```
  async function async1() {
      console.log("async1 start");
      await  async2();
      console.log("async1 end");
   }
  async  function async2() {
     console.log( 'async2');
     await async3();
  }
  async function async3() {
     console.log('async3')
  }
  console.log("script start");
  setTimeout(function () {
      console.log("settimeout");
  },0);
  async1();
  new Promise(function (resolve) {
      console.log("promise1");
      resolve();
  }).then(function () {
      console.log("promise2");
  });
  console.log('script end');

```
script start
async1 start
async2
promise1
script end
promise2
async1 end
undefined
setTimeout

async 表达式的返回值
await表达式的作用和返回值

await是一个让出线程的标志，await后面的函数会先执行一遍， 然后就会跳出整个async函数来执行后面的js代码
等本轮事件循环执行完了之后，又会跳回到async函数中等待await
后面表达式的返回值，如果返回值为非Promise则继续执行async函数后面的代码，否则将返回promise放入队列中



```
  async function async1() {
      console.log("async1 start");
      await  async2();
      console.log("async1 end");
   }
  async  function async2() {
     console.log( 'async2');
     await async3();
  }
  async function async3() {
     console.log('async3')
     await callApi();
     console.log('async3 end'); 
  }
const callApi = () => {
return new Promise(function(resolve, reject){
  resolve(2);
}).then(result => console.log('async3 promise result', result))

}
  console.log("script start");
  setTimeout(function () {
      console.log("settimeout");
  },0);
  async1();
  new Promise(function (resolve) {
      console.log("promise1");
      resolve();
  }).then(function () {
      console.log("promise2");
  });
  console.log('script end');

```
结果：
```
script start
async1 start
async2
async3
promise1
script end
async3 promise result 2
promise2
async3 end
async1 end
undefined
settimeout
```
