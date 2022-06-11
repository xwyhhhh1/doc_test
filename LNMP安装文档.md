## web集群搭建培训

### 一、mysql的配置文件对比

### 二、nginx

```shell
yum -y install pcre pcre-devel oenssl openssl-devel gcc gcc-c++ libaio-devel
useradd nginx -s /sbin/nologin
./configure --user=nginx --group=nginx --prefix=/applicattion/nginx-1.6.3 --with-http_stub_status_module --with-http_ssl_module
make
make install
ln -s /applicattion/nginx-1.6.3 /applicattion/nginx
```

### 三、mysql

```shell
mv mysql-5.5.62-linux-glibc2.12-x86_64 /applicattion/mysql-5.5.32
ln -s /applicattion/mysql-5.5.32/ /applicattion/mysql
mkdir -p /applicattion/mysql/data/
chown -R mysql.mysql /applicattion/mysql
useradd mysql -s /sbin/nologin 
/applicattion/mysql/scripts/mysql_install_db  --basedir=/applicattion/mysql --datadir=/applicattion/mysql/data/ --user=mysql

echo "export PATH=/applicattion/mysql/bin:$PATH" >> /etc/profile
source /etc/profile
/bin/cp support-files/my-small.cnf /etc/my.cnf
cp support-files/mysql.server /etc/init.d/mysqld
sed -i  's#/usr/local/mysql#/applicattion/mysql#g' /applicattion/mysql/bin/mysqld_safe /etc/init.d/mysqld 
/etc/init.d/mysqld start
```

### 四、PHP

```shell
yum -y install zlib-devel libxm12-devel libjpeg-devel libjepg-turbo-devel libconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel mhash libmcrypt-devel mcrypt
```

```shell
cd /usr/local/src
http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz
tar zxf libiconv-1.14.tar.gz
cd libiconv-1.14
./configure --prefix=/usr/local/libiconv
#报错
#参考文章https://blog.csdn.net/tianya_lu/article/details/103265210
cd ..
tar -xf php-5.3.17.tar.gz
./configure --prefix=/applicattion/php-5.3.27 --with-mysql=/applicattion/mysql --with-iconv-dir=/usr/local/libiconv --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpate --enable-safe-mode --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --with-curlwrappers --enable-mbregex --enable-fpm --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --enable-short-tags --enable-zend-multibyte --enable-static --with-xsl --with-fpm-user=nginx --with-fpm-group=nginx --enable-ftp
make && make install
ln -s /applicattion/php-5.3.27/ /applicattion/php
cp php.ini-production /applicattion/php/lib/php.ini
cd /applicattion/php/etc/
cp php-fpm.conf.default php-fpm.conf
/applicattion/php/sbin/php-fpm 
```

### 五、nginx与php结合配置

```shell
cd /applicattion/nginx/conf/
vim nginx.conf
#添加配置在http段
include extra/blog.conf
mkdir -p /applicattion/nginx/conf/extra
vim blog.conf
#添加内容
server {
    listen        80;
    server_name   blog.etiantian.org;
    location / {
        root   html/blog;
        index  index.html index.htm;
    }
    location ~ .*\.(php|php5)?$ {
         root   html/blog;
         fastcgi_pass   127.0.0.1:9000;#此为处理php规则，交于本机9000端口处理
         fastcgi_index  index.php;
         include  fastcgi.conf;#此为交由fastcgi处理的规则
    }
}
mkdir -p /applicattion/nginx/html/blog
cd /applicattion/nginx/html/blog
echo "http://blog.etiantian.org" > index.html
echo "<?php phpinfo(); ?>" >test_info.php
cd /applicattion/nginx/sbin/
./nginx -s reload
#配置域名解析访问，http://ip/test_info.php，访问结果如下
```

![image-20220515171916640](http://xwyhhhh1.test.upcdn.net/image-20220515171916640.png)

```shell
#测试连接mysql情况
cd /applicattion/nginx/html/blog
vim test_mysql.php
<?php
      $link_id=mysql_connect('localhost','root','!QAZ2wsx') or mysql_error();
      if ($link_id){
           echo "mysql successful by xiao !";
      }else{
          echo mysql_error();
      }
?>

```

### 六、配置wordpress

```shell
mysql -uroot -p #loging
grant all on wordpress.* to wordpress@'localhost' identified by '123456';
create database wordpress;
#查看
use mysql;
select host,user,password from user;
show grants for wordoress@'localhost';
flush privileges;
#添加支持php首页
cd /applicattion/nginx/conf/extra
vim blog.conf
server {
    listen        80;
    server_name   blog.etiantian.org;
    location / {
        root   html/blog;
        index  index.php index.html index.htm;
    }
    location ~ .*\.(php|php5)?$ {
         root   html/blog;


         fastcgi_pass   127.0.0.1:9000;
         fastcgi_index  index.php;
         include  fastcgi.conf;
    }
}
cd /usr/local/src
#上传wordpress至/usr/local/src下
#解压
tar -xf ......#解压安装包
mv wordpress/* /applicattion/nginx/html/blog/
#删除多余文件
rm -rf index.html
rm -rf test_mysql.php
rm -rf test_info.php
#修改所有者
chwon -R /applicattion/nginx/html/*
/applicattion/nginx/sbin/nginx -s reload
#访问blog.etiantian.com完成配置
```

![image-20220515183112708](http://xwyhhhh1.test.upcdn.net/image-20220515183112708.png)

![image-20220515183131750](http://xwyhhhh1.test.upcdn.net/image-20220515183131750.png)







