---
title: call, apply, bind的区别
date: 2018-12-01 18:25:08
tags: javascript
---

call/apply/bind改变函数运行时候的上下文

* bind 的实现
```
Function.prototype.bind = function(obj) {
  var method = this;
  var args = [].slice.call(arguments, 1);
  return function() {
    method.apply(obj, args.concat([].slice.call(arguments)));
  }
}
```

* bind/apply/call的区别
bind: 只是改变了this指向，没有立即调用
apply: 传入的参数需要是数组，立即调用
call:  传入正常的参数，立即调用

* 使用场景
```

var people = {
  sayName: function(age){
    debugger
    console.log(this.name, age);
  }
};
var person = {
  name: 'marong'
}

people.sayName.call(person, 22);
people.sayName.bind(person)(22);
people.sayName.apply(person, [22]);
```
