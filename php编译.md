## php源码编译

###一. 简述

```shell
#本人编译php纯粹为了学习，和思考生产环境应该如何定制较好，事实上学会这项技能更多的是用在搭建模拟生产环境上，因为很少有公司会特别找人编译，除了新公司，老公司一般都有自己的已经搭好的平稳框架，并不需要去进行更改什么的。即使是新公司，也可以通过购买云主机定制化安装，因此此项能更多的是用在模拟公司生产环境，和学习等。
```

#### 1.1 php版本

![image-20220511152637439](http://xwyhhhh1.test.upcdn.net/image-20220511152637439.png)

#### 1.2 php源码包

```shell
#网址php.net
```

![image-20220511152737571](http://xwyhhhh1.test.upcdn.net/image-20220511152737571.png)

![image-20220511152952045](http://xwyhhhh1.test.upcdn.net/image-20220511152952045.png)

#### 1.3 libzip版本

```shell
#编译php7.4.24版本php需要libzip，yum安装的libzip版本过低会报出lib版本过低错误，因此需要先编译libzip
```

![image-20220511153837794](http://xwyhhhh1.test.upcdn.net/image-20220511153837794.png)

#### 1.4 libzip源码包

```shell
#网址https://libzip.org/
```

![image-20220511154652755](http://xwyhhhh1.test.upcdn.net/image-20220511154652755.png)

### 二. 安装

#### 2.1环境准备

```shell
#创建www用户，在编译php时会指定运行php的用户，并且为了安全考虑不允许www用户拥有登录权限
useradd nginx -s /sbin/nologin
#配置阿里的centos源和epel扩展源
#...此项略过...
#安装依赖
yum -y install gcc gcc-c++ gd gd-devel libxml2-devel unzip sqlite-devel libcurl libcurl-devel oniguruma-devel pcre-devel perl perl-devel wget vim
```

#### 2.2准备安装包

```shell
#libzip源码包
cd /usr/local/src
wget https://libzip.org/download/libzip-1.2.0.tar.gz
#php源码包
cd /usr/local/src
wget https://www.php.net/distributions/php-7.4.24.tar.gz
```

#### 2.3编译libzip

```shell
#解压libzip源码包
cd /usr/local/src
tar -xf libzip-1.2.0.tar.gz
#编译生成MAKEFILE,指定安装在/usr/local/libzip
cd libzip-1.2.0
./configure --prefix=/usr/local/libzip
make && make install
#在全局变量中指定libzip下的pkconfig，否则php的编译项里--with-zip或其它项会找不到依赖或者找之前老的依赖
export PKG_CONFIG_PATH="/usr/local/libzip/lib/pkgconfig/" 

################
未见报错即为成功
一般情况下不会报错
这是已经验证过的
################
```

#### 2.4编译php

```shell
#解压php源码包
cd /usr/local/src
tar -xf php-7.4.24.tar.gz
#编译生成MAKEFILE,指定安装在/usr/local/php
cd php-7.4.24
--prefix=/usr/local/php --with-fpm-user=nginx --with-fpm-group=n --with-zlib --with-pdo-mysql=mysqlnd  --enable-mysqlnd --with-curl --with-jpeg --with-xpm --enable-fpm --enable-ftp --enable-gd --enable-mbstring --enable-sockets  --with-zip --with-pcre-jit
make && make install

################
未见报错即为成功
一般情况下不会报错
这是已经验证过的
################
```
