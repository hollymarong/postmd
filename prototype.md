---
title: prototype
date: 2017-04-24 08:12:45
tags: JavaScript
---
构造函数，原型，实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，实例都包含一个指向原型对象的内部指针。
原型链：一个对象的原型等于另一个对象的实例，此时原型对象将包含一个指向另一个原型的指针，层层递进，构成了实例与原型之间的链条，这就是原型链。
继承：依靠原型链来实现的。
1. 原型链继承
很少单独使用
缺点：最主要的问题来自包含引用类型值的原型会被所有实例共享;创建子类型的实例时，不能向超类型传递参数
```javascript
function SuperType(){
    this.colors = ['red', 'blue', 'green'];
}
function SubType(){
}
SubType.protoype = new SuperType();
var instance1 = new SubType();
instance1.colors.push('black');
console.log('colors', instance1.colors); //['red', 'blue', 'green', 'black']
var instance2 = new SubType();
console.log('colors', instance2.colors); //['red', 'blue', 'green', 'black']

```
2. (经典继承)借用构造函数继承
很少单独使用
在子类构造函数的内部调用超类构造函数,解决引用类型共享的问题；可传递参数
缺点：无法实现函数的复用
```javascript
function SuperType(name){
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}
function SubType(){
    SuperType.call(this, name);
}
var instance1 = new SubType();
instance1.colors.push('black'); //['red', 'blue', 'green', 'black']
console.log(instance1);
var instance2 = new SubType();
console.log(instance2); // ['red', 'blue', 'green']
```

3. 组合继承(原型链和借用构造函数的组合)
将原型链和借用构造函数的技术组合到一块，使用原型链实现对原型属性和方法的继承，通过借用构造行数来实现对实例属性的继承,既通过在原型上定义方法实现了函数复用，又能保证每个实例儿都有自己的属性
常用的继承模式
缺点： 无论什么情况下，都会调用两次超类型构造函数
```
function SuperType(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
}

SuperType.prototype.sayName = function() {
  console.log('sayName', this.name);

}

function SubType(name, age) {
  SuperType.call(this, name); // 第二次调用
  this.age = age;
}

SubType.prototype = new SuperType(); // 第一次调用
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function() {
  console.log(this.age);
}

var instance1 = new SubType('Nicholas', 29);
instance1.colors.push('black'); // ['red', 'blue', 'green', 'black']
console.log('instance1.colors', instance1.colors);
instance1.sayName(); // Nicholas
instance1.sayAge(); // 29

var instance2 = new SubType('Greg', 27);
console.log('instance2.colors', instance2.colors); // ['red', 'blue', 'green']
instance2.sayName(); // Greg
instance2.sayAge(); // 27
```

4. 原型式继承(同ECMAScript的Object.create)
基于已有的对象，借助原型创建新对象
应用场景：只想让一个对象与另一个对象保持类似的情况下。
缺点：包含引用类型的值会共享
Object.create() 与 object()方法行为相同
```
function object(o) {
  function F() {}
  F.prototype = o;
  return new F();
}

var person = {
  name: 'Nicholas',
  friends: ['shellby', 'Court', 'Van']
};
var anotherPerson = object(person);
anotherPerson.name = 'Greg';
anotherPerson.friends.push('Rob');

var yetAnotherPerson = object(person);
yetAnotherPerson.name = 'Linda';
yetAnotherPerson.friends.push('Barbie');

console.log('person.name', person.name); // Nicholas
console.log('person.friends', person.friends); // ["shellby", "Court", "Van", "Rob", "Barbie"]

```
5. 寄生式继承
创建一个仅用于封装继承过程的函数，任何能够返回新对象的函数都适用于此模式
缺点：不能做到函数的复用
```javascript
function createAnother(original) {
  var clone = object(original);  // 通过调用函数创建一个新对象
  clone.sayHi = function() {  // 以某种方式来增强这个对象
    console.log('hi');
  }
  return clone; // 返回这个对象
}
var person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van']
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); // hi
```
6. 寄生组合式继承
最常用的继承模式
借用构造函数来继承属性，通过原型链来继承方法。
与组合式继承相比,它只调用了一次SuperType构造行数，并且因此避免了在SubType.prototype上创建不必要的，多余的属性。寄生组合式继承是引用类型最理想的继承方式
```javascript
function inheritPrototype(subType, superType) {
  var prototype = Object.create(superType.prototype);
  prototype.constructor = subType;
  subType.prototype  = prototype;
}

function SuperType(name) {
  this.name = name;
  this.colors = ['red', 'blue', 'green'];
}

SuperType.prototype.sayName = function() {
  console.log(this.name);
}

function SubType(name, age) {
  SuperType.call(this, name);
  this.age = age;
}

inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function() {
  console.log(this.age);
}

var person = new SubType('xiaoming', 28);
person.sayAge(); // 28
person.sayName(); // xiaoming
```
