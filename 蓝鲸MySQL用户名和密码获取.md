## 蓝鲸MySQL用户名和密码获取

1.搜索MySQL所在的服务器

2.搜索登录MySQL的用户名和密码/data/install/bin/04-final/usermgr.env

3.查询MySQL的socket，路径/etc/mysql/default.my.conf 

4.登录mysql ：  mysql -uroot -pm6wBBlgq8Uqg -S /var/run/mysql/default.mysql.socket

5.创建新用户并授权：grant all on 数据库.表 （全部用*代替） ‘xwy’@'%' identified by '密码'； flush privileges;

6.DBeaver 工具进行远程连接测试







jXKGLUrqQuX