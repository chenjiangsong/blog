---
title: js中的数组化
date: 2016-09-08 19:04:18
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

### 思考？`Array.prototype.slice.call`的应用范围
在jQuery的数组化过程中我们看到了剔除了带有length属性的window、string、function，我们也在`Array.prototype.slice.call`里试一试

![](http://ob3wg7deo.bkt.clouddn.com/14715012487175.jpg)

很明显`Array.prototype.slice.call`没有经过考验，把带有length属性的对象都slice成了数组。

### 总结

**类数组对象**

- arguments
- HTMLCollection
- 自定义对象arrayLike

**带有length属性的对象**

- 数组
- window对象
- 字符串
- function

**`Array.prototype.slice.call`是通过判断参数有没有length属性来slice数组的**
