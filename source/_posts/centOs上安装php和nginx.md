---
title: centOs上安装php和nginx
date: 2015-12-12 16:02:02
tags: linux
---

## 安装编译环境
```bash
yum install -y gcc gcc-c++ ncurses-devel perl
```

----
## 安装pcre开发包（nginx依赖）
```bash
yum install -y pcre-devel
```

----
## 安装zlib
```bash
yum install -y zlib-devel
```
<!-- more -->
----
## 安装Nginx
```bash
wget http://nginx.org/download/nginx-1.7.9.tar.gz

tar xzvf nginx-1.7.9.tar.gz

cd nginx-1.7.9

./configure --prefix=/usr/local/nginx

--user=nobody --group=nobody && make && make install
```
>wget:下载工具命令tar:解压缩包命令 参数：-x 解压 -z 判断是否使用gzip压缩 -v 解压过程显示文件 -f 使用文档名

----

## 启动nginx
```bash
/usr/local/nginx/sbin/nginx
```

----
## 设置nginx开机启动
**写入文件**

```bash
vim /etc/init.d/nginx
```
**设置成可执行**

```bash
chmod +x /etc/init.d/nginx
```
**设置成开机启动**

```bash
chkconfig nginx on
```
----
## 安装PHP依赖包
```bash
yum -y install autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel libmcrypt libmcrypt-devel mysql-devel
```

----
## 生成libmysqlclient软链
```bash
ln -s /usr/lib64/mysql/libmysqlclient.so.18.0.0 /usr/lib/libmysqlclient.so
```
>通过查找libmysqlclient,发现是在/usr/lib64/mysql/目录内的libmysqlclient.so.18.0.0做的软连接,PHP默认是去的 /usr/lib/搜索,所以没有找到，所以要在php默认的/usr/lib/目录下生成链接到libmysqlclient的软链。
>**注意，这里的libmysqlclient.so.18.0.0可能更新而修改版本号（之前我参考的教程就是libmysqlclient.so.16.0.0），所以我们要先进入/usr/lib64/mysql/目录，确定这个文件（libmysqlclient.so.18.0.0）的具体名称，再生成软链。**

----
## 安装libmcrypt
>centos源不能安装libmcrypt-devel，由于版权的原因没有自带mcrypt的包。所以这里我们使用自己在网上下载的包，然后上传到服务器的目录上。

```bash
cd /usr/local
```

```bash
wget http://softlayer.dl.sourceforge.net/sourceforge/mcrypt/libmcrypt-2.5.8.tar.gz (使用附件的包或者自己下载的包)
```

```bash
tar -zxvf libmcrypt-2.5.8.tar.gz
```

```bash
cd libmcrypt-2.5.8
```

```bash
./configure --prefix=/usr/local && make && make install
```

----
## 安装PHP
```bash
wget http://cn2.php.net/distributions/php-5.3.29.tar.gz
```

```bash
tar xzvf php-5.3.29.tar.gz
```

```bash
cd php-5.3.29
```

**内存大于等于1G时**

```bash
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-mcrypt --enable-mbstring --with-curl --disable-debug -with-bz2 --with-zlib --enable-sockets --enable-zip --with-pcre-regex --with-mysql --with-mysqli --with-gd --with-jpeg-dir && make && make install
```

**内存小于1G时**

```bash
./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --with-mcrypt --enable-mbstring --with-curl --disable-debug -with-bz2 --with-zlib --enable-sockets --enable-zip --with-pcre-regex --with-mysql --with-mysqli --with-gd --with-jpeg-dir --disable-fileinfo && make && make install
```

```bash
cp php.ini-production /usr/local/php/etc/php.ini
```

```bash
ln -s -f /usr/local/php/bin/phar.phar /usr/local/php/bin/phar
```

```bash
cd /usr/local/php/etc/
```

```bash
mv php-fpm.conf.default php-fpm.conf
```

```bash
mkdir /usr/local/php/log
```

```bash
chmod 777 /usr/local/php/log
```

----
## 修改php-fpm.conf文件

```bash
vim /usr/local/php/etc/php-fpm.conf
```
>设置错误日志路径 **error_log = /usr/local/php/log/php_errors.log**

----
## 修改php.ini文件
```bash
vim /usr/local/php/etc/php.ini
```
>设置时区中国 **date.timezone = PRC**
>隐藏php版本号 **expose_php=Off**

----
## 启动php-fpm
```bash
cd /usr/local/php-5.3.29
```

```bash
cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
```

```bash
chmod +x /etc/init.d/php-fpm
```

```bash
chkconfig php-fpm on
```

```bash
/etc/init.d/php-fpm start
```

----
## 配置一个虚拟主机
```bash
mkdir /usr/local/nginx/conf/vhosts
```

```bash
vim /usr/local/nginx/conf/nginx.conf
```
>去掉log_format main前的注释，在最后一个“}”前加入一行 **include vhosts/*.config;**

----
## 检查一下配置文件是否有问题
```bash
/usr/local/nginx/sbin/nginx -t
```
>**正确响应：**
>nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
>nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful

----
## 重新加载配置文件
```bash
/usr/local/nginx/sbin/nginx -s reload
```
>每次新写入一个/usr/local/nginx/conf/vhosts/*.conf 文件都要运行这个命令哦
