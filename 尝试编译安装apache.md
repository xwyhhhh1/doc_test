## 尝试编译安装apache

#### 1、到Apache官网获得安装包

+ 官网地址为https://apache.org/

#### 2、安装依赖

+ apr和apr-utli

+ pcre（正则扩展库）

  + 安装pcre需要c语言编译器

  ```shell
  yum install -y gcc gcc-c++
  ```

  





















































yum install -y gcc gcc-c++

apache不支持pcre2，

./configure --prefix=/usr/local/apache --enable-so --enable-rewrite --enable-ssl --enable-modules=most --enable-mpms-shared=all --with-mpm=event --with-included-apr