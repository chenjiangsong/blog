---
title: 单行文字两端对齐：用css伪类实现
date: 2016-10-09 16:36:45
tags: css
---

今天在做项目的时候碰到这个问题：**右侧红线内的小标题单行两端对齐。**
![设计稿](http://ob3wg7deo.bkt.clouddn.com/14760023432680.jpg)
两端对齐的css属性我们知道是`text-align:justify`，但是这个属性有它的局限性：
1.`display`只能为`inline`或`inline-block`
2.多行文字才能实现两端对齐。
也就是说，在现在的情况下，小标题的单行文字仅仅使用`text-align:justify`是实现不了两端对齐的。
那我们就想办法把这个『单行文字』变成『多行文字』

怎样让一块内容后面多出东西，而又不会使页面多出垃圾元素呢？嘿嘿，类比清除浮动用到的方法，我们可以很快就想到使用`:after`伪类来解决问题。
<!-- more -->
html

```html
<div class="small-unit">
    <label class="label">开发</label>
    <div class="msg">阿斯顿</div>
</div>
<div class="small-unit">
    <label class="label">开发商</label>
    <div class="msg">阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿</div>
</div>
<div class="small-unit">
    <label class="label">开发周期</label>
    <div class="msg">阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿阿斯顿</div>
</div>
```

css

```css
.small-unit{
    padding: 10px 0;
}

.label{
    position: absolute;
    display: inline-block;
    width: 4em;
    height: 40px;
    text-align: justify;
    overflow: hidden;
}

.label:after{
    display: inline-block;
    content: '';
    width: 4em;
    height: 40px;
    text-align: justify;
    overflow: hidden;
}

.msg{
    display: inline-block;
    margin-left: 5em;
    position: relative;
}

.msg:before{
    content: '：';
    position: absolute;
    left: -1em;
}

```

效果图，完美！
![](http://ob3wg7deo.bkt.clouddn.com/14760665741148.jpg)


## 注意点
1.`.label`和它的`after`伪类要部分相同的css属性，来保证`after`元素是`.label`的第二行

```
{
    display:inline-block;
    text-align: justify;
    overflow: hidden;
    width: 4em;
    height: 40px;
}
```
2.`:`冒号推荐写在`.msg`元素的`before`伪类上，写在label里面或外面都不能满足需求，具体效果可以自己试试看

```
<label>{{label}}：</label>
或
<label>{{label}}</label>：
```
3.最后，将`.label`元素用绝对定位固定住，顺便实现右侧多行的效果。

