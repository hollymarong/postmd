---
title: Javascript相等操作符
date: 2017-05-22 10:26:34
tags: JavaScript
---
相等操作符分为两种，一种是相等(==)和不相等(!=)，另一种是全等(===)和不全等(!==)。
相等和不相等操作符进行比较的时候，如果数据类型不相同，会强制类型转换，再进行比较
全等和不全等操作符进行比较的时候，不会进行类型转换，直接比较

不同数据类型转换规则
* 布尔值转换为0或1
* 字符串和数字比较，字符串会转化为数字
* 对象和其他类型比较，对象会调用valueOf或toString方法，得到的基本类型值，按照之前转化规则进行转化后，比较
* null和undefined是相等的, null和undefined不会转化为其他类型值
* NaN 不等于NaN
* 如果两个对象比较，如果两个对象指向同一个对象，则相等，否则，不相等

| 表达式                  | 值    | 原因                                            |
| ----                    | ---   | ---                                             |
| null == undefined       | true  |                                                 |
| NaN == NaN              | false | NaN不等于NaN                                    |
| 0 == false              | true  | false转换为0                                    |
| 1 == true               | true  | true转换为1                                     |
| 2 == true               | true  | true转换为1, 1不等于2                           |
| [] == false             | true  | []调用toString转化为"",""字符串转换为0          |
| [1] == 1                | true  | [1]调用toString转化为"1","1"字符串转换为1       |
| [] == 0                 | true  | []调用toString转化为"",""字符串转换为0          |
| {} == false             | false | {}调用toString->"[object Object]"->NaN,false->0 |
| {} == "[object Object]" | true  | {}调用toString转换为"[object Object]",          |
| {} == 0                 | false |                                                 |
| {} == false             | false | {}调用toString->"[object Object]"->NaN, NaN==0  |
| {} == {}                | false | 两个对象,分别保存在不同的内存地址中             |
| [] == []                | false | 两个对象，分别保存在不同的内存地址中            |
| [1] == [1]              | false | 两个对象,分别保存在不用的内存地址中             |

```javascript
var obj = {
    toString: function(){
        return 10;
    }
};
obj == 10 //true

var obj = {
    valueOf: function(){
        return 20;
    }
};
obj == 20 // true
var obj = {
    toString: function(){
        return 10;
    },
    valueOf: function(){
        return 20;
    }
};
obj == 20 //true
```
由此可见，对象会先调用valueOf试图转换成基本类型值, 如转换不成会再尝试调用toString方法。

