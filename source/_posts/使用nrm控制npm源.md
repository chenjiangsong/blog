---
title: 使用nrm控制npm源
date: 2016-07-30 10:17:17
tags: npm
---

nrm是一个npm源管理器，安装之后使用命令行操作可以方便地在各个npm源之间切换。并且自带了新增自定义npm源，测速等功能
<!-- more -->

### 安装
```
npm install -g nrm
```
### 列出已有npm源
```
nrm ls  //带*号的是当前使用的源
```
![](http://ob3wg7deo.bkt.clouddn.com/QQ20160730-1@2x.png)
### 切换
```
nrm use taobao //切换到taobao源
```
![](http://ob3wg7deo.bkt.clouddn.com/QQ20160730-2@2x.png)

![](http://ob3wg7deo.bkt.clouddn.com/QQ20160730-3@2x.png)

已经成功切换到taobao源 这时候执行`npm i`命令就会从taobao源上下载包并安装

### 新增
```
npm add <registry> <url> [home]
```

![](http://ob3wg7deo.bkt.clouddn.com/QQ20160730-4@2x.png)

### 删除
```
nrm del <registry>
```
![](http://ob3wg7deo.bkt.clouddn.com/QQ20160730-5@2x.png)

### 测单一速度
```
nrm test <registry>
```
### 测全部速度
```
nrm test
```
![](http://ob3wg7deo.bkt.clouddn.com/QQ20160730-6@2x.png)

### github
[github.com/Pana/nrm](github.com/Pana/nrm)
