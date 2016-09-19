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

### typeof
```js
typeof null // object
```

typeof不能辨别`null`与对象
