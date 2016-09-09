---
title: crontab入门
date: 2016-07-25 15:13:03
tags: linux
---

crontab是linux系统下的定时任务，可用于定时执行脚本，实现定时爬虫、任务等功能。
本文简单介绍了命令行的crontab操作。

<!-- more -->
### 简单的crontab命令：

>crontab -help
>crontab -l   ：查看已有的定时任务
>crontab -e  ：编辑或新增定时任务
>crontab -r   ：删除此用户下所有定时任务

### 一条crontab任务的结构

```
   *      *      *      *      *               command
|      |      |      |      |                  |
分钟    小时    天     月      周         运行的语句、执行的程序
(0~59) (0~23) (1~31) (1~12)  (0~6)
```
 >在以上各个字段中，还可以使用以下特殊字符：
 >
星号（*）：代表所有可能的值。

>逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”

>中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”

>正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。



### 已经写好的实例（天气预报爬虫）

>1,5,6,7,8 8,11,18 * * * /usr/local/php/bin/php /usr/local/project/htdocs/app_rpt/index.php weather/airquality spyForecast >/dev/null 2>&1

>9,13,14,15,16 8,11,18 * * * /usr/local/php/bin/php /usr/local/project/htdocs/app_rpt/index.php weather/airquality spyIndex >/dev/null 2>&1

>55 7,10,17 * * * /usr/local/php/bin/php /usr/local/project/htdocs/app_rpt/index.php weather/airquality set0 >/dev/null 2>&1

>\*/5 * * * * /usr/local/php/bin/php /usr/local/project/htdocs/app_rpt/index.php airpm/get_latest >/dev/null 2>&1

>55 23 * * * /usr/local/php/bin/php /usr/local/project/htdocs/app_rpt/index.php weather/airquality calculate >/dev/null 2>&1

### 自己动手操作
>\*/1 * * * * date >> /usr/local/project/htdocs/app_pc/resource/log.txt >/dev/null 2>&1

>将当前服务器的时间输入log.txt，每分钟执行一次


### 查看crontab日志
>进入日志目录  cd  /var/log

>只查看crontab相关日志文件  ls -l cron*

### 特殊技法
四月第一个周日执行任务

```bash
30 19 1-7 4 *  test \`date +\%w` -eq 0 && [command]
```

### 参考资料
[慕课网 crontab教程](http://www.imooc.com/learn/216)
