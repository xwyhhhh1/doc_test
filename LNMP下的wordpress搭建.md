## LNMP下的wordpress搭建

#### 1.Nginx安装部署

+  访问nginx官网，http://nginx.org，点击downloa，找到stable and mainline，点击进去，找到官方给出的搭建仓库的配置
   + ![image-20220214203653059](http://xwyhhhh1.test.upcdn.net/image-20220214203653059.png)
   + ![image-20220214203934144](http://xwyhhhh1.test.upcdn.net/image-20220214203934144.png)
+ 搭建好之后直接yum安装即可

#### 2.mysql部署

+ 我们选择mariadb

   + ```shell
      yum -y install mariadb-server
      # 无密码回车即可
      mysql -uroot -p
      # 创建一个数据用户和一个数据库并且赋予权限
      create user 'wordpress'@'localhost' identified by '1234';
      create database wordpress;
      grant all on wordpress.* to 'wordpress'@'localhost' indentified by '1234';
      quit
      ```

#### 3.部署php

+ 安装php-fpm以及相关插件

   + ```shell
      yum -y install php php-fpm
      ```

#### 4.下载wordpress

+ 将其上传到机器中，解压

+ 修改配置

   + ```shell
      # 默认是没有配置文件的，所以需要将重命名
      mv wp-config-sample.php wp-config.php
      # 修改以下信息
      vim wp-config.php
      /** WordPress数据库的名称 */
      define('DB_NAME', 'ceshi');
      
      /** MySQL数据库用户名 */
      define('DB_USER', 'wordpress');
      
      /** MySQL数据库密码 */
      define('DB_PASSWORD', 'xwy1234!');
      
      /** MySQL主机 */
      define('DB_HOST', 'localhost');
      
      /** 创建数据表时默认的文字编码 */
      define('DB_CHARSET', 'utf8');
      
      /** 数据库整理类型。如不确定请勿更改 */
      define('DB_COLLATE', '');
      
      ```

#### 5.整合配置

+ 首先是nginx，我们需要让nginx支持php，并将他的首页文件替换成wordpress
   + 