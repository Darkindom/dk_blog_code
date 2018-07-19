---
title: JS数据类型笔记
date: 2018-07-19 18:15:29
tags:
- Javascript
categories:
- 前端
description: javascript的数据类型整合笔记
---

# 数据类型概要
5种基本数据类型：Undefined,Null,Boolean,Number,String
1种复杂数据类型：Object
typeof 返回值：undefined, boolean, string, number, function, object。null返回object，正则返回function, 因为Null其实是一个空对象指针

# 基本数据类型
## Number
* 八进制第一位必须是0，如果超出范围，第一位0无效，当做十进制。十六进制 0x。进行计算时，都转换成十进制。
* 对浮点数：如果小数点后没有带数字，或者本身就是一个整数，都会转换成整数。
* 科学计数法 3e-10 等于 3x10^(-10)。 浮点数做运算，注意精确度。比如 0.1+0.2不等于0.3
* 无穷 Infinity -Infinity Number.MAX_VALUE Number.MIN_VALUE
* NaN 任何涉及NaN的操作都会返回NaN，NaN与任何值不相等包括NaN本身。

## String
字符串一旦创建，不能改变值。每次都销毁原来的字符串再重新填充新的。

## Object
可以通过new创建，new 如果不给构造函数传参数，可以省略括号
```new Object;```

* Object属性
  * constructor
  构造函数，保存用于创建当前对象的函数
  * hasOwnProperty(propertyName)
  检查属性在不在当前对象实例中（不是实例的原型）
  * isPrototypeOf(object)
  检查传入对象是否是当前对象的原型
  * propertyIsEnumberable(propertyName)
  检查给定属性能否用for-in语句枚举
  * toLocalString(), toString()
  返回对象的字符串表示
  * valueOf()
  返回对象的字符串、数值或者布尔值表示，通常与toString相同

# 引用类型
## Object
创建方式    1.new Object()     2.对象字面量
访问属性 1.点表示法 .xxx     2.方括号表示法 [‘xxx’]

## Array
创建方式 1.new Array()      2.数组字面量
可以通过length属性 移除数组的项 或者添加新项
sort()方法比较的是字符串，会调用每项的toString，所以要比较数值的话，传入比较函数
indexOf() lastIndexOf()   接受2参数，要查找的项和查找起点。一个从头开始找，一个从尾开始。查找使用的是全等操作符
迭代 every() filter() forEach() map() some() 
     接受2参数 要运行的函数，运行该函数的作用域对象-影响this值
          运行的函数接受3参数 数组项的值，位置，数组对象本身
every()  函数对每一项都返回true的话，返回true
some()   函数对任一项有返回true的话，返回true
filter() 返回数组，数组由函数得true的项组成
map()    返回所有函数结果组成的数组
forEach()无返回，所有项运行函数
归并 reduce() reduceRight() 迭代所有项，构建一个最终返回的值。reduceRight()就是从数组尾开始遍历数组
接受2参数 要运行的函数，归并基础的初始值
函数接受4参数 前一个值，当前值，项的索引，数组对象。函数返回的任何值都会作为第一个参数自动传给下一项

## Date
创建方式 new Date() 默认当前时间，
如果要指定时间，new Date(Date.parse())或new Date(Date.UTC())。也可以直接加，相当于默认使用parse()
Date.parse() 接受一个表示日期的字符串参数，返回相应日期的毫秒数。
Date.UTC() 接受参数分别是年，月(0-11)，日，小时，分钟，秒，毫秒。年月必须有，日默认1，其他默认0。
Date.now() 返回调用时的日期和时间的毫秒数。
toDateString()   显示星期几 月 日 年
toTimeString()   显示时 分 秒 时区
toUTCString()    显示完整格式的UTC日期

## 正则RegExp
Perl语法    /pattern/flags    flags: g全局， i不区分大小写，m多行模式    元字符：() [] {} \ ^ $ | ? * + .    使用元字符需要转义
RegExp构造函数    接受2参数，匹配的字符串模式pattern，可选的flags
.global 布尔值 是否设置了g标志  .ignoreCase 布尔值 是否设置了i标志  .lastIndex 整数，开始搜索下一个匹配项的字符位置，从0算起
.multiline 布尔值 是否设置了m标志   .source 正则表达式的字符串表示
方法    .exec()  .test()
.exec()  接受1参数 要应用模式的字符串，返回包含第一个匹配项信息的数组，没有匹配项返回null。
返回的数组包含属性index，input。 index表示匹配项在字符串中的位置，input表示应用正则表达式的字符串
数组中  第一项是整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串
即使设置全局g标志，exec每次也只返回一个匹配项。不设置的话永远返回第一个匹配项，设置的话每次都从上次匹配位置后开始。
.test()  接受1参数 字符串。模式与该参数匹配的话返回true，否则false。
.toString  返回正则表达式的字面量
属性
```
input        $_    最近一次要匹配的字符串
lastMatch    $&    最近一次的匹配项
lastParen    $+    最近一次匹配的捕获组
leftContext    $`    input字符串中lastMatch之前的文本
rightContext    $’    input字符串中lastMatch之后的文本
multiline    $*    布尔值，是否所有表达式都使用多行模式
```

## 函数     没有重载
创建
 * 1.函数声明语法 function f(){...}
 * 2.函数表达式 var f = function(){...};
 * 3.使用Function构造函数（这也是一种函数表达式，但不推荐）
  （注意函数表达式要加分号，就像声明其他变量一样）  

函数声明与函数表达式的区别
 * 解析器会率先读取函数声明，并使其在执行任何代码前可用
 * 函数表达式必须等到解析器执行到它所在的代码行，才会真正被解释执行。



- - -
# 判断类型
* typeof x
判断x的类型 string number boolean undefined object
* x instanceof constructor
判断x是不是类型constructor
* isFinite(x)
是不是有穷的
* isNaN(x)
任何不能被转换成数值的值 返回true
先把值转换成数值，如果是对象 调用valueOf()，再调用toString()
* Array.isArray(x)
判断是否是数组


# 转换类型
* Boolean(x) true,false
  * Number 非零数字(包括无穷大), 0和NaN
  * Undefined N/A, undefined
  * String 非空字符串, ""
  * Object 任何对象，null

* Number(x) 1,0
  * Boolean
true,false    null 0       undefined NaN
  * String 
如果只含数字或者有效的浮点格式，转成对应的数值。（忽略前导0）；
如果包含有效的十六进制格式 0x 转成对应的十进制； 
如果是空的，转成0
如果不是上述情况，转成NaN

* parseInt(x, 10)，parseFloat(x)
  * 用于转 string 为 number，后面是进制，parseFloat只解析十进制
忽略字符串前的空格，从第一个非空格开始，第一个字符不是数字或负号，返回NaN。
直到解析完所有后续字符 或者 遇到一个非数字字符。0开头 8进制，0x开头 16进制
小数点 parseInt无效，parseFloat第二个小数点无效，parseFloat如果结果是整数，返回整数
e.g. ’1234blue’->1234 ‘’->NaN

* String(x)
  * 如果值有.toString方法，直接调用.toString()方法
如果是null, undefined 返回”null” “undefined”

* .toString(10)    参数是进制
  * null 和 undefined 没有这种方法

* valueOf() 返回对象的字符串、数值或者布尔值表示，通常与toString相同


## 数组方法
push()      队尾加，返回长度
pop()          队尾移除，返回移除掉的项
shift()      队头移除，返回移除掉的项
unshift()    队头加，返回长度
reverse()    反转，返回排序后的数组
sort()        排序，返回排序后的数组
concat()    连接，创建新数组
splice(p,n,r)    p操作的位置，n删除的数量，r插入的项(任意数量)
slice()        接受2参数，起始和结束。返回截取部分，不会影响原数组。
reduce()    归并 详见Array部分
reduceRight()    归并 详见Array部分

.every()      函数对每一项都返回true的话，返回true
.some()       函数对任一项有返回true的话，返回true
.filter()     返回数组，数组由函数得true的项组成
.map()        返回所有函数结果组成的数组
.forEach()    无返回，所有项运行函数