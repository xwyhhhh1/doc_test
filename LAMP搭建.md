## LAMP搭建

#### 1、Apache的安装

+ 相较于编译安装，直接yum安装也是个不错的方式

  + ```shell
    yum -y install httpd
    ```

+ 添加配置文件添加一台虚拟主机

  > Apache中会自动读取位于/etc/httpd/conf.d下的以.conf为结尾的文件，因此并不需要对配置文件做出更改

  + ```shell
    vim /etc/httpd/conf.d/ixdba.conf
    <VirtualHost *:80>
    ServerAdmin admin@ixdba.net
    ServerName www.ixdba.net
    ServerAlias www
    DocumentRoot /data/www/html/ixdba
    
    <Directory "/data/www/html/ixdba">
        Options FollowSymLinks
        AllowOverride ALL
        Require all granted
    </Directory>
    
    
    ErrorLog /var/log/httpd/ixdba_error.log
    CustomLog /var/log/httpd/ixdba_access.log combined
    </VirtualHost>
    
    ```

  + ```shell
    mkdir -p /data/www/html/ixdba#上面指定了虚拟主机网页文件存放地址，因此需要创建
    ```

+ 启动Apache服务

  + ```shell
    systemctl start httpd#启动服务
    systemctl enable httpd#设置开机自启
    ```

#### 2、数据库的部署

+ 这里的数据库选择mariaDB，他与mysql基本操作也没什么太大的区别

+ 安装mariaDB

  + 因为mariaDB在比较新的版本中都自带安装包所以直接yum安装

  + ```shel
    yum -y install mariadb mariadb-server
    ```

  + ```shell
    systemctl restart maiadb
    systemctl enable mariadb
    ```

+ 使用内置脚本开启安全模式，修改密码

  + ```shell
    /usr/bin/mysql_secure_installation #建议使用内置脚本的绝对路径
    #刚开始会让你输入密码，因为没有密码所以直接回车
    #第二个会询问你是否设置root管理员的密码，y后面会让你设置密码和再次输入
    #第三个是询问你是否删除一个名为anonymous的用户，删除即可
    #第四个是询问你是否关闭root用户的远程登陆权限，为了安全考虑建议y
    #第五个是询问是否删除测试数据库及其权限y即可
    #第六个是问你是否冲重新加载授权表，y即可
    ```

#### 3、快速安装配置PHP

+ 也是yum安装但是需要安装一些相关的模块

  + ```shell
    yum -y install php php-cli php-pear php-pdo php-mysqlnd php-gd php-mbstring php-mcrypt php-xml
    #如果只是单纯的安装php并不需要安装很多模块，但是一般安装php都是要配合其他软件一起使用
    ```

##### 至此，LNMP基本环境搭建完毕，但是要知道基本上，这个环境是为了应用而服务的，如果只是这样搭建出来而什么应用都不去装的话，那他只是个环境什么都做不了

#### 4、安装phpMyadmin

+ phpMyadmin通过提供web页面的方式使用操作数据库，十分便利快捷

+ yum安装

  + ```shell
    yum -y install phpmyadmin
    ```

+ 修改配置文件

  + ```shell
    vim /etc/httpd/conf.d/phpMyAdmin.conf
    #将所有的Allow from 127.0.0.1  Allow from ::1 替换成Require all granted
    systemctl restart httpd
    ```

#### 5、部署WordPress程序

+ 首先下载https://cn.wordpress.org/download/#download-install

  + 上传到linux里解压，放到/data/html/ixdba下

+ 修改运行apache服务的用户

  + ```shell
    useradd ixdba
    passwd ixdba
    ```

  + ```shell
    vim /etc/httpd/conf/httpd.conf
    #在66、67行修改这两个参数
    User ixdba
    Group ixdba
    ```

+ 赋予权限

  + ```shell
    chown -R ixdba:ixdba /data/www/html/ixdba
    ```

+ 创建存储wordpress的数据库

  + ```shell
    mysql -uroot -p #指定登录用户，以交互式密码登录
    ```

  + ```maria
    create database cmsdb;#创建一个数据库
    grant all on cmsdb.* to 'ixdba' identified by 'password';将数据库权限赋予用户ixdba，并设置密码
    flush privileges;#更新权限，类似于刷新
    quit#‘优雅的’推出数据库
    ```

  + ```shell
    systemctl restart httpd
    ```

  + 访问ip+wordpress，在没有做域名解析的情况下即可



#### 6、总结

+ 安装部署apache，添加虚拟主机
+ 安装mariadb，运行内置脚本开启安全模式设置密码
+ 安装php和一些插件
+ 安装phpmyadmin这个web端的管理数据库的工具
+ 修改相应配置
+ 下载wordpress
+ 添加新用户ixdba，更改apache服务用户，更改wordpress文件用户权限
+ 创建数据库，赋予权限给用户ixdba
+ 结束

