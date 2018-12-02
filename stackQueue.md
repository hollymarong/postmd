---
title: stackQueue
date: 2018-07-22 23:03:59
tags: javascript
---
## 栈
栈是一种特殊的列表，栈内的元素只能通过栈顶访问, 是一种后入先出（LIFO, last-in-first-out）的数据结构, 例如摞盘子就是常见的栈的列子。

## 队列
队列是一种特殊的列表，只能在队尾插入元素，在对首删除元素，是一种先进先出(FIFO, first-in-first-out)的数据结构。


## 用栈实现队列

思路：
入队：将元素压入栈1
出队：如果栈2 为空，将栈1倒(pop)入(push)栈2, 栈2出栈(pop);
     如果栈2 不为空， 栈2 出栈;
```
function Stack() {
  this.dataStore = [];
  this.push = function(item) {
    this.dataStore.push(item);
    return this.dataStore;

  };
  this.pop = function() {
    return this.dataStore.pop();
  };
  this.length = function() {
    return this.dataStore.length;
  }
};


function Queue() {
  var stack1 = new Stack();
  var stack2 = new Stack();
  this.enqueue= function(item) {
    return stack1.push(item);
  };
  this.dequeue = function() {
    if(!stack1.length() && !stack2.length()){
      throw new Error("Queue is empty");
    }
    if(!stack2.length()) {
      while(stack1.length()) {
        stack2.push(stack1.pop());
      }
    }
    return stack2.pop();
  };
};

var queue = new Queue();
console.log(queue.enqueue(2))
console.log(queue.enqueue(4))
console.log(queue.enqueue(6))
console.log(queue.dequeue())
console.log(queue.enqueue(8))
console.log(queue.enqueue(10))
console.log(queue.dequeue())
console.log(queue.dequeue())
console.log(queue.dequeue())
console.log(queue.dequeue())

```
## 用队列实现栈
要实现的功能，一个数组，后进先出，也就是只能用push，shift来实现push, pop功能
思路：
进栈：将元素插入当前有值的队列 
出栈：将当前有值的队列，将该队列中除最后一个元素的全部元素依次出队，返回该对的最后一个元素。
```
function Queue() {
  this.dataStore = [];
  this.enqueue = function(item) {
    this.dataStore.push(item);
    return this.dataStore;
  }
  this.dequeue = function() {
    return this.dataStore.shift();
  }
  this.length = function() {
    return this.dataStore.length;
  }
}

function Stack() {
  var queue1 = new Queue();
  var queue2 = new Queue();
  this.push = function(item){
    let curQueue = queue1;
    curQueue = !!queue2.length() ? queue2 : queue1;
    return curQueue.enqueue(item);
  }
  this.pop = function(){
    if(!queue1.length() && !queue2.length()) {
      throw new Error('Stack is Empty');
    }
    const curQueue = !!queue1.length() ? queue1 : queue2;
    const tmpQueue = curQueue === queue1 ? queue2 : queue1;

    while(curQueue.length() > 1) {
      tmpQueue.enqueue(curQueue.dequeue());
    }
    return curQueue.dequeue();

  }
}

var stack = new Stack();
console.log(stack.push(2));
console.log(stack.push(4));
console.log(stack.push(6));
console.log(stack.pop());
console.log(stack.push(8));
console.log(stack.push(10));
console.log(stack.pop());
console.log(stack.pop());
console.log(stack.pop());
console.log(stack.pop());

```
