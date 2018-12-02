---
title: define定义模块函数的实现
date: 2018-11-03 17:36:59
tags: javascript
---
```
var Amd = (function Manager(){
  var modules = {};
  function define(name, deps, impl) {
    for(var i = 0; i < deps.length; i++) {
      deps[i] = modules[deps[i]];
    }
    modules[name] = impl.apply(impl, deps);

  }
  function get(name) {
    return modules[name];
  }
  return {
    define:define,
    get: get,
  }
})();


Amd.define('a', [], function(){
  function sayName(who) {
    return 'Hello ' + who;
  }
  return {
    sayName,
  };
});

Amd.define('b', ['a'], function(a) {
  var name = 'test';
  function sayInfo() {
    console.log(a.sayName(name));
  }
  return {
    sayInfo,
  }
})

var b = Amd.get('b');
b.sayInfo();
```
