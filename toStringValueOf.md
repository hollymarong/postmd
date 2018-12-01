---
title: toString和valueOf的用法
date: 2017-03-24 09:33:02
tags: JavaScript
---
数值、布尔值、字符串和对象都有toString(),valueOf方法。
null和undefined没有这两个方法。
toString():返回对象的字符串表示
valueOf():返回对象的字符串、数值或布尔值表示。
在javascript中，很多操作都会在后台隐式调用对象的toString()和valueOf()方法,以便取得可操作的值。
内置函数和操作符的一般执行流程：首先会调用对象的valueOf方法，然后确定该方法返回的值是否可以按照相应的规则转换，如果不能，则基于这个返回值再调用toString()方法，再操作返回的字符串。

| type      | valueOf  | toString       |
|-----------|----------|----------------|
| undefined | 无此方法 | 无此方法       |
| null      | 无此方法 | 无此方法       |
| boolean   | boolean  | "true"/"false" |
| string    | string   | string         |
| object    | object   | string         |
| Date      | number   | string         |
| function  | function | string         |

***
## toString和valueOf的应用
应用1：利用输入函数名，会隐式调用valueOf(),toString()方法实现add()()()...

```javascript
function add(){
    var result = 0;
    var args = [].slice.call(arguments);
    args.forEach(function(num){
        result += num;
    });
    var fn = function(){
        var args = [].slice.call(arguments);
        args.push(result);
        return add.apply(null,args);
    };
    fn.valueOf = fn.toString = function(){
        return result;
    };
    return fn;
}
add(1,3) //4
add(1)(3) //4
add(1,3)(1)(3) //8

//方法二，给函数添加result属性，用来保存每次计算的结果
function add(){
    var args= [].slice.call(arguments);
    var result = 0;
    args.forEach(function(num){
       result += num;
    });
    result += ~~add;
    add.result = result;
    return add;
}
add.valueOf = add.toString = function(){
    return add.result;
};
add(1,3) //4
//以后每次调用需要重置result的值
add(1)(3) //4
```
