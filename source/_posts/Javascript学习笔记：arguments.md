---
title: Javascript学习笔记：arguments
date: 2015-11-05 13:58:25
tags: 学习笔记
hide: true
---


1.ES函数不介意传递进来多少参数，因为参数在函数内部是用一个数组：`arguments`（准确说是『类数组』）来表示的
2.`arguments`可以使用方括号语法来访问每一个元素，并且有length属性
3.`arguments`对象可以与命名参数一起使用

```js
function doAdd(num1, num2) {
    if (arguments.length === 1) {
        alert(num1)
    } else if (arguments.length === 2) {
        alert(arguments[0] + num2)
    }
}
doAdd(10)      // 10
doAdd(10, 20)  // 30
```
4.`arguments`的值永远与对应命名参数的值保持同步

```js
function doAdd(num1, num2) {
    arguments[1] = 10
    alert(arguments[0] + num2)
}
doAdd(10, 20)   // 20
```
5.`arguments`对象的长度是由参数个数决定，不是由定义函数时的命名参数个数决定
6.`arguments`转成数组用`Array.prototype.slice.call`方法

```js
function doAdd() {
    var args = Array.prototype.slice.call(arguments)
    console.log(arguments)
    console.log(args)
}
doAdd(1,2,3,4)
```
![](http://ob3wg7deo.bkt.clouddn.com/14783263408259.jpg)
7.`callee`属性
arguments对象有callee属性， 该属性是一个指针，指向拥有这个arguments对象的函数。在递归函数中，推荐使用`arguments.callee`

```js
//阶乘函数

//bad
function factorial(num) {
    if (num <= 1) {
        return 1
    } else {
        return num * factorial(num - 1)
    }
}

//good
function factorial(num) {
    if (num <= 1) {
        return 1
    } else {
        return num * arguments.callee(num - 1)
    }
}
```
防止对factorial函数赋值到另一个函数名时，内部递归调用factorial失败

