---
title: CORS：跨域解决方案
date: 2015-10-19 22:24:44
tags: 前端
---

CORS是一种新的服务端跨域方案。实现方式非常简单。

优点：相比jsonp，可以支持http的所有请求方式，jsonp只支持get方式（因为本质是通过script标签的src属性访问url）。支持ajax。
缺点：低版本浏览器（IE9-）不支持。
<!-- more -->

客户端js：

```js
var xhr = XMLHttpRequest()
xhr.open('post',url,true) //url绝对路径，true异步，false同步
xhr.withCredentails = true
xhr.onload = function(){}
xhr.send()
```

服务端(node)

```js
res.setHeader('Access-Control-Allow-Origin',req.header.origin);
res.setHeader('Access-Control-Allow-Credentails',true);//告诉客户端可以在http请求中加上cookie
res.setHeader('Access-Control-Allow-Method','POST,GET,PUT,DELETE,OPTIONS')
```


