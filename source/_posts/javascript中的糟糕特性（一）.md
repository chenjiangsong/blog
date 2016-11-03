---
title: javascript中的糟糕特性（一）
date: 2016-09-19 22:38:42
tags: javascript
---

无论前端的大牛还是小白，在与人讨论各个语言的特性时，逢JavaScript必谈其语言缺陷。然而作为一个混迹前端两年的老菜鸟，仍对大家伙口耳相传的『JavaScript语言缺陷』了解甚少。最近看技术稍微注意了一些，在博客中将其记录下来。在更深入地了解这门『吃饭手艺』的同时，也警示自己远离JavaScript的这些糟糕特性，编写出更有质量的代码。
<!-- more -->
### 全局变量
定义全局变量有3种方式

1.在任何函数外放置一个`var`语句

```js
var foo = value
```
2.给全局对象`window`添加一个属性

```js
window.foo = value
```
3.**隐式全局变量**——未经生命的变量

```js
foo = value
```
坑爹的就是第三种定义方式。JavaScript的策略是让那些忘记预先声明的变量成为全局变量，本意是为了方便初学者，然而在代码质量上会造成**全局变量污染**，在业务处理过程中，也会导致**bug难以定位**

### 自动插入分号
大括号换行还是不换行的选择困难患者的福音。

JavaScript有一个自动修复机制，它试图通过自动插入分号来修正有缺损的程序。这在某种情况下会造成更严重的错误。
当我们使用`return`返回一个对象时

```js
return
{
  status: true
}
//通过自动插入分号机制会变成==>
return ;
{
  status: true
};
```
就是`return`了一个`undefined`！这可不是我们想要的结果！

正确的保护`return`语句不被**自动插入分号机制**坑害的写法：

```js
return {
  status: true
}
```

### 混乱的类型判断
js是[弱类型语言](http://baike.baidu.com/link?url=XieOWMuLKgWn8skVS_Vegl21mJFj5eYcK2zasna-DfhO5e7ChsCFx9Zfch8s8wGgHEoDPzl_yn-AfVE2MgPb5Md7dLvavbyJTsvk7FlA1KrHjsGNckSfLL2nPFeU0-IbZtJOp78-MskShi5QEhK3c_)，相对于[强类型语言](http://baike.baidu.com/link?url=_cfMaG15PXrgQIbVoxWVLtZz5LKNYe_-CQT0XIvMUwTJ4qaH14v-HDffD_PNO9pQPqO51HlDlG43nCtpNEoU2tNOwjdabnAlwcha4qo699_yTU9GvciLIDZXNVCTVOxi1bwOzUY12Ycv8bRKS3pIia)它有两个特性：
1.不必严格定义数据类型
2.不必先定义后使用

虽然使用的时候方便灵活，不用进行类型转换，也不用定义多个变量，但是对于需要严谨的工作态度的研发人员来说，这其实是非常混乱、极易出错的特性。

正因为数据类型很『随性』，所以js带来了好几种**判断数据类型**的方案，然而都不是很完美
#### typeof
typeof是最简单常见的数据类型判断工具了，它可以快速的判断出`number`，`boolean`，`string`，`undefined`，`object`，甚至是`function`，
但是它居然辨别不出**null**与**对象**！

```js
typeof null // object
```
同样也分辨不出**数组**和**对象**，

```js
typeof [] //object
```

**正则**与**对象**呢？

```js
typeof /a/ //object  注：Safari5、Chrome7及之前的版本会返回 function
```

好吧，同样是object里的复杂数据类型，你就把**function**单独判断出来是几个意思。。

```js
typeof function(){}   // function  
```
太坑爹了。。

#### instanceof
instanceof能判断出是否是我们想要的数据类型，然而获取不到变量的数据类型
#### Object.prototype.toString.call()
应该是判断数据类型最强大的方法了，虽然它的返回值不够简便，而且调用方法太长了。
不过我们可以自己封装一个简便的方法放到自己的util工具集里

```js
const getDataType = function(obj) {
    return Object.prototype.toString.call(1).replace(/(\[object\s)(\w+)(\])/,'$2')
}
const a = 1
const b = '呵呵哒'
const c = []
const d = {}
const e = function(){}
const f = /abcd/
const g = null
getDataType(a)   // Number 
getDataType(b)   // String
getDataType(c)   // Array
getDataType(d)   // Object
getDataType(e)   // Function
getDataType(f)   // RegExp
getDataType(g)   // Null
```
甚至能检测出 `arguments`

```js
var testArgument = function() {
    return getDataType(arguments)
}
testArgument() // Arguments
```

### 浮点数
打开浏览器的控制台，输入以下几个例子，大家感受下
![](http://ob3wg7deo.bkt.clouddn.com/14781041293673.jpg)
糟糕点：
1.js浮点数计算精度丢失，**所以尽量不要在前端进行计算处理！丢给后端去做！**
2.js浮点数连整数带小数部分只有17个有效数字，**所以精度过高的浮点数在前端这里展示都成问题！**


