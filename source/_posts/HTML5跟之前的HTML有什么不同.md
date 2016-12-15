---
title: HTML5跟之前的HTML有什么不同
date: 2016-12-05 15:17:04
tags: html
---

*HTML*是超文本标记语言。所谓超文本，就是图片、链接、音乐甚至程序。HTML5（以下简称*HTML5*）和HTML4.01（以下简称*HTML*）及以前的版本有着根源上的差别。
提到标记语言就不得不提*SGML*（标准通用标记语言）。SGML提供了标记语言的语法和规范。*XML*是SGML的一个子集，HTML是SGML的一个应用。
我们先来聊聊XML。XML全称是*可拓展标记语言*，意思就是在双方约定好的前提规范下，标签名是可以自由定义的。这个*约定好的前提规范*就是DTD（文档规则定义）。
<!-- more -->
```xml
<?xmlversion="1.0"?>
<!DOCTYPE note[
<!ELEMENT note(to,from,heading,body)>
<!ELEMENT to(#PCDATA)>
<!ELEMENT from(#PCDATA)>
<!ELEMENT heading(#PCDATA)>
<!ELEMENT body(#PCDATA)>
]>
<note>
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don'tforgetmethisweekend</body>
</note>
```
这里的`<!doctype note>`则是规定了`note`为xml文档的根元素，就像HTML的doctype规定了`html`是文档的根元素一样`<!doctype html>`
接下来的就是DTD，规定了`note`,`to`,`from`,`heading`,`body`这几个标签元素的作用（是用来传递数据的）

```xml
<!ELEMENT note(to,from,heading,body)>
<!ELEMENT to(#PCDATA)>
<!ELEMENT from(#PCDATA)>
<!ELEMENT heading(#PCDATA)>
<!ELEMENT body(#PCDATA)>
```
同样，在HTML4.01及之前的版本中，doctype后面也是跟了DTD。我们打开[http://www.w3.org/TR/html4/strict.dtd](http://www.w3.org/TR/html4/strict.dtd)可以看到对HTML各个标签的详细定义。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```
基于此，我们可以浅显地定义一下，HTML4.01及之前的版本是适用一套通用DTD的XML文档。
那为什么HTML5的概念会被提出呢？HTML5跟之前的HTML又有什么区别呢？
我们还是来看一下XML，XML是被用来传递数据的，HTML是处理超文本的，处理超文本这件事不符合SGML的规范，所以在HTML5之前，基于SGML的HTML4.01是无法处理`video`、`audio`等超文本元素的。所以W3C为了网页应用更好的发展，制定了不基于SGML的新的HTML5规范，在沿用老版本功能的同时，也放弃了SGML的种种限制。自然而然，DTD这个SGML的产物也就不需要再出现在HTML5文档的头部了。可以说，HTML5能推出新标签，得益于脱离了SGML规范这个本质。

