---
title: javascript学习笔记：获取页面节点种数
date: 2016-11-05 15:21:39
tags: 学习笔记
hide: true
---
一行代码：

```js
new Set([...document.all].map(e => e.tagName)).size
```

