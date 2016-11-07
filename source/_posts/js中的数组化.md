---
title: js中的数组化
date: 2016-06-08 19:04:18
tags: javascript
---

浏览器下有很多**类数组对象**

- arguments
- HTMLCollection（document.getElementsByTagName等方式获取的节点集合）
- 特殊写法的自定义对象arrayLike，如下：
<!-- more -->

```js
var arrayLike = {
  0:'a',
  1:'b',
  2:'c',
  length:3
}
```

先用`Object.prototype.toString.call()`检测一下这三种类数组对象的数据类型

```js
Object.prototype.toString.call(arguments) //[object Arguments]
Object.prototype.toString.call(document.getElementsByTagName('div')) // [object HTMLCollection]
Object.prototype.toString.call(arrayLike) // [object Object]
```
### 数组化的三种方法
#### Array.prototype.slice.call
类数组对象的数组化 最简单的方法是 `Array.prototype.slice.call`。但在旧版本IE下的HTMLCollection、NodeList不是object下的子类，浏览器会报异常，所以来看看jquery是怎么实现的

```js
//jQuery makeArray
var makeArray = function(array) {
  var ret = []
  if (array != null) {
    var i = array.length
    //window对象 字符串 函数 也有length属性
    if (i == null || typeof array === 'String' || jQuery.isFunction(array) ||array.setInterval)
        ret[0] = array
    else
        ret[--i] = array[i]
  }
  return ret
}
```
主要原理是获取类数组的length属性的值，然后将类数组里的元素全都push到一个新的真实数组里去。
#### Array.from（ES6）
`Array.from`方法用于将两类对象转为真正的数组：类数组对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。

```js
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).forEach(function (p) {
  console.log(p);
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
//字符串
Array.from('hello')   // ['h', 'e', 'l', 'l', 'o']

//Set对象
let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```
#### **...**（ES6）
`...`是ES6中新增的扩展运算符，也可以将某些数据结构转为数组。

```js
// arguments对象
function foo() {
  var args = [...arguments];
}

// NodeList对象
[...document.querySelectorAll('div')]
```
### 思考？数据化跟length属性有关么？
在jQuery的数组化过程中我们看到了剔除了带有length属性的window、string、function，我们也在`Array.prototype.slice.call`里试一试

![](http://ob3wg7deo.bkt.clouddn.com/14715012487175.jpg)

很明显`Array.prototype.slice.call`没有经过考验，把带有length属性的对象都slice成了数组。

实际上，js中的几种数组化的方法，都是调用遍历器接口`Symbol.iterator`，如果一个对象没有部署，就无法转换。
**`Array.prototype.slice.call`和`Array.from`还支持转换类数组对象（本质特征就是有length属性），也就是说任何有length属性的对象都可以通过这两种方法转换成数组，而`...`不行。**
**并且`Array.prototype.slice.call`和`Array.from`的返回结果还有一些不同**

```js
var a = { length: 2 }

[...a]
Array.from(a)
Array.prototype.slice.call(a)
```

![](http://ob3wg7deo.bkt.clouddn.com/14785121041337.jpg)

我们可以看到：
1.`...`不能直接转换`{length:2}`这种对象。
2.`Array.from`返回一个100个成员的数组，并且每个成员都为`undefined`
3.`Array.prototype.slice.call`返回一个length为100的数组，没有定义数组成员

### 总结

**类数组对象**

- arguments
- HTMLCollection
- 自定义对象arrayLike

**js数组转换的方法**


- Array.prototype.slice.call
- Array.from
- ...

**带有length属性的特殊对象**

- 数组
- window对象
- 字符串
- function

**三种方法都是优先判断对象有没有遍历器接口（Symbol.iterator）来进行数组转换，`Array.prototype.slice.call`和`Array.from`还可以通过判断参数有没有length属性来转换数组**


