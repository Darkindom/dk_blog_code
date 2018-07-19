---
title: JS原型链学习笔记
date: 2018-07-19 17:43:41
tags:
- Javascript
- JS原型链
categories:
- 前端
description: 学习JS原型链的笔记
---

# 先上结论
```
person1.__proto__ == Person.prototype

Person.__proto__ === Function.prototype
Object.__proto__ === Function.prototype
Function.__proto__ === Function.prototype

Person.prototype.__proto__ == Object.prototype
Function.prototype.__proto__ == Object.prototype
Object.prototype.__proto__ == null
```

## 结论
- 一. 凡是通过 new Function() 创建的对象都是函数对象，其他的都是普通对象。

- 二. person1 和 person2 都是 构造函数 Person 的实例
一个公式：实例的构造函数属性（constructor）指向构造函数。

- 三. 每个对象都有 __proto__ 属性，但只有函数对象才有 prototype 属性。
（普通对象的__proto__ 指向它的构造函数的prototype； 函数对象的__proto__全都指向 Function.prototype）
原型对象（Person.prototype）是 构造函数（Person）的一个实例。
PS:  Function.prototype 它是函数对象，但它很特殊，他没有prototype属性        console.log( typeof Function.prototype.prototype ) //undefined
    Function.prototype 为什么是函数对象呢？
            var A = new Function ();        Function.prototype = A;

- 四. JS 在创建对象（不论是普通对象还是函数对象）的时候，都有一个叫做__proto__ 的内置属性，用于指向创建它的构造函数的原型对象。
person1.__proto__ == Person.prototype
这个连接存在于实例（person1）与构造函数（Person）的原型对象（Person.prototype）之间，而不是存在于实例（person1）与构造函数（Person）之间。

- 五. 
- 六. 
- 七. 所有的构造器都来自于 Function.prototype，甚至包括根构造器Object及Function自身。所有构造器都继承了·Function.prototype·的属性及方法。如length、call、apply、bind
Function.prototype.__proto__ === Object.prototype
Object.prototype.__proto__ === null // true

- 八. 所有函数对象的 __proto__ 都指向 Function.prototype，它是一个空函数（Empty function）
 Object 的每个实例都具有以上的属性和方法。所以我可以用 Person.constructor     也可以用 Person.hasOwnProperty。
Object.getOwnPropertyNames    获取所有（包括不可枚举的属性）的属性名不包括 prototy 中的属性，返回一个数组

- 九. 给Person.prototype赋值的是一个对象直接量{getName: function(){}}，使用对象直接量方式定义的对象其构造器（constructor）指向的是根构造器Object，Object.prototype是一个空对象{}，{}自然与{getName: function(){}}不等。

- 十. Object.__proto__ === Function.prototype // true
  Function.__proto__ === Function.prototype // true
   Function.prototype.__proto__ === Object.prototype //true

- 十一. 
  - 原型和原型链是JS实现继承的一种模型。
  - 原型链的形成是真正是靠__proto__ 而非prototype

> 在默认情况下，所有的原型对象都会自动获得一个 constructor（构造函数）属性，这个属性（是一个指针）指向 prototype 属性所在的函数（Person）
> 
> 一个普通对象的构造函数 === Object
> 
> Object.prototype 对象也有proto属性，但它比较特殊，为 null 。因为 null 处于原型链的顶端，这个只能记住。


1. person1.__proto__ 是什么？
2. Person.__proto__ 是什么？
3. Person.prototype.__proto__ 是什么？
4. Object.__proto__ 是什么？
5. Object.prototype__proto__
6. http://www.jianshu.com/p/652991a67186
```
function f1(){}; 
var f2 = function(){};
var f3 = new Function('str','console.log(str)');     //以上均为函数对象

var A = new Person();
Person.prototype = A;

Person.prototype.constructor == Person;

person1 = new Person();
person1.__proto__ == Person.prototype;
person1.constructor == Person;

Object.prototype.__proto__ === null

//所有的构造器都来自于Function.prototype，甚至包括根构造器Object及Function自身（Array String Number Boolean Error Date RegExp）
Object.__proto__ === Function.prototype  // true
Object.constructor == Function // true
Function.__proto__ === Function.prototype // true
Function.constructor == Function //true

Function.prototype.__proto__ == Object.prototype
Object.prototype.__proto__ == null    // 到顶了
```

# 对象的基本

## 对象创建方法
- 1. new
- 2. 对象字面量


_year  属性前有_下划线，是一种常用的记号，表示只能通过对象方法访问的属性

两对方括号 表示特性是内部值，不能直接访问它们。  有两种属性：数据属性 和 访问器属性
| 数据属性 ||
|---|---|
| [[Configurable]] | 默认true，表示能否通过delete删除，能否修改属性特性，能否把属性修改为访问器属性|
| [[Enumerable]] | 默认true，表示能否通过for-in循环返回属性|
| [[Writable]] | 默认true，表示能否修改属性的值|
| [[Value]] | 包含这个属性的数据值|

|访问器属性||
|---|---|
| [[Configurable]] ||
| [[Enumerable]] ||
| [[Get]] | 默认undefined，在读取属性时调用的函数|
| [[Set]] | 默认undefined，在写入属性时调用的函数|

# 创建对象
- 1. 工厂模式
- 2. 构造函数模式
构造函数始终以一个大写字母开头，而非构造函数以一个小写字母开头
可以将自定义的构造函数的实例标识为一种新的特定类型，用 instanceof 检测。
构造函数没有显式创建对象；直接将属性和方法赋给了this对象；没有return语句。
  * 缺点：每个方法都要在实例上重新创建一遍。
  * 解决：把方法的函数定义转移到构造函数外。

- 3. 原型模式
创建每个函数都有一个原型属性（prototype），是一个指针，指向一个原型对象。
原型对象prototype 包含可以由特定类型的所有实例共享的属性和方法。
原型对象会自动获得一个constructor（构造函数）属性，指向prototype属性所在函数的指针。
通过这个constructor 可以继续给原型对象添加其他属性和方法。
当调用构造函数创建一个新实例后，该实例内部将包含一个指针（内部属性）指向构造函数的原型对象。可以通过 `__proto__`属性访问。
【实例中的指针指向原型，而不是构造函数！】
这个连接是存在于实例与原型对象之间的，不是存在于实例与构造函数之间的。
每次读取某个对象的某个属性时，先从实例对象开始，如果实例中有该属性，则返回该属性；没有则搜索它的原型对象。
实例中的同名属性会屏蔽原型中的属性。使用delete可以删除实例属性，重新访问原型中的属性。
如果原型对象使用对象字面量来重写，那么这个prototype的constructor属性，就不会再指向原型对象了，而是指向Object构造函数。
所以需要重新让constructor 指向原型对象。
但是以这种方式重设constructor属性，会使constructor属性的 [[Enumerable]] 特性变成true，（原生constructor属性是不可枚举的）
所以可以使用 Object.defineProperty 的方法。
对原型所做的修改都能立即反映在实例上。
把原型修改为另外一个对象 就等于 切断了构造函数和最初原型的联系。变成了一个新的原型。所以实例也就被切断了和新的原型的联系。
  * 优点: 使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。
  * 缺点:
    - 1. 所有实例在默认情况下都将取得相同的属性值
    - 2. 最大问题 由其共享的本性导致。对于引用类型。

- 4. 构造函数模式与原型模式组合            使用最广泛、认同度最高的一种创建自定义类型的方法。
构造函数模式用于定义实例属性，原型模式用于定义方法和共享属性。

- 5. 动态原型模式
通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。
Ps: 使用动态原型模式时，不能使用对象字面量重写原型，不然会切断现有实例和新原型之间的联系。

- 6. 寄生构造函数模式
模式和工厂模式一模一样，只是封装创建对象的代码，返回新创建的对象。

- 7. 稳妥构造函数模式
没有公共属性
不引用this对象；     不使用new调用构造函数。

# 原型链
## 确定原型和实例的关系
```
instanceof
  instance instanceof Object        //true
  instance instanceof SuperType     //true
isPrototypeOf
  Object.prototype.isPrototypeOf(instance)        //true
  SuperType.prototype.isPrototypeOf(instance)     //true
```
通过原型链继承时，
* 重写原型链中已经存在的方法，会屏蔽原来的方法。
* 不能使用对象字面量创建原型的方法，这样会重写原型链。

问题：
- 通过原型实现继承时，原型实际上会变成另一个类型的实例。于是原先的实例属性也就变成了现在的原型属性。
- 创建子类型的实例时，不能向超类型的构造函数中传递参数。

所以有了以下
## 1. 借用构造函数（也叫 伪造对像 经典继承）
在子类型构造函数的内部调用超类型构造函数。
```
function SubType() {
     SuperType.call(this);
}
```
优点：可以向超类型构造函数传递参数
缺点：
  - 1 函数复用
  - 2 在超类型的原型中定义的方法，对子类型而言是不可见的

## 2. 组合继承 （也叫 伪经典继承）
将原型链和借用构造函数的技术组合起来。思路：使用原型链实现对原型属性和方法的继承，通过借用构造函数实现对实例属性的继承。


## 3. 原型式继承
不必创建自定义类型，本质上object() 对传入其中的对象执行了一次浅复制。
```
function object(c) {
     function F() {};
     F.prototypr = o;
     return new F();
}
```
规范化
- object.create()
- 接受2参数，用作新对象原型的对象，一个为新对象定义额外属性的对象（额外属性会覆盖原型对象上的同名属性）


## 4. 寄生式继承
待补充


### 5. 寄生组合式继承
通过原型链的混成形式来继承方法，通过借用构造函数来继承属性。
背后的基本思路：不必为了指定子类型的原型而调用超类型的构造函数。本质上就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

# 方法：
```
Object.defineProperty()              修改属性默认的特性，
  接受3参数 属性所在的对象，属性的名字，一个描述符对象
  如果没有指定描述符对象，则configurable enumerable writable的属性默认为false。如果有描述符则无此限制
  可以多次调用defineProperty()，但是一旦设置 [[Configurable]]为false，就只能修改 writable的特性。

Object.defineProperties()            定义多个属性特性。
  接受2参数 1.属性所在的对象，2.第二个对象的属性与第一个对象中要添加或修改的属性一一对应。

Object.getOwnPropertyDescriptor()    读取属性的特性    
  接受2参数  属性所在的对象， 要读取其描述符的属性名称 。    
  返回一个对象，如果是数据属性，有configurable enumerable writable value，如果是访问器属性，有configurable enumerable get set

.isPrototypeOf( )                    判断是否是原型对象
比如 alert( Person.prototype.isPrototypeOf( person1 ) )    //true

.getPrototypeOf( )                   返回这个对象的原型
比如 alert( Object.getPrototypeOf( person1 ) == Person.prototype )    //true
  
delete                               删除某个属性
比如  delete person1.name

.hasOwnProperty( )                   检测属性是否存在于实例
比如  alert( person1.hasOwnProperty( "name" ) )            //false

in                                   能否通过对象访问给定属性，无论是在实例还是原型中
比如   alert( "name" in person1 )        //true

Object.keys( )                       获得可枚举的实例属性
比如 alert( Object.keys( Person.prototype ) )                //"name","age","sayName"

Object.getOwnPropertyNames( )        获得所有实例属性（包括不可枚举的）      
比如 alert( Object.getOwnPropertyNames( Person.prototype ) )    //"constructor","name","job","sayName"
```






