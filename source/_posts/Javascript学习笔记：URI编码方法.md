---
title: Javascript学习笔记：URI编码方法
date: 2015-11-05 14:37:09
tags: 学习笔记
hide: true
---
### 编码
encodeURI()和encodeURIComponent()方法都可以对URI编码发送给浏览器。
#### 为什么要编码？
因为有效的URI不能包含某些字符，例如空格。比如给后端接口传递当前页面的url时，包含一些特殊字符可能会在传递过程中或者接收之后解析不了导致失败
#### 有啥区别
encodeURI()只将`空格`转义成`%20`，适用于整个URI，不会将例如`http://`里的冒号斜杠都转义
encodeURIComponent()会将所有非字母数字都进行编码，使用于URI中的某一段（例如hash）
### 解码
decodeURI()和decodeURIComponent()分别对应上面两种编码方式

