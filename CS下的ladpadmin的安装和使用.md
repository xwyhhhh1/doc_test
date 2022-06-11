## C/S下的ladpadmin的安装和使用

#### 下载安装包、解压

+ https://sourceforge.net/projects/ldapadmin/files/ldapadmin/1.8.1/LdapAdminExe-w64-1.8.1.zip/download
  + 下载好之后是一个压缩包
  + 可解压也可不解压，不解压双击进去就是一个.exe的文件；解压之后也是出现一个.exe的文件，双击进入

#### 连接ldap服务器

+ 进去之后是这个界面

  ![image-20220120163210140](C:%5CUsers%5C%E8%93%9D%E6%AD%A3%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20220120163210140.png)

+ 点击Start之后再点击connect.....就会出现这样的界面

  ![image-20220120163420037](C:%5CUsers%5C%E8%93%9D%E6%AD%A3%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20220120163420037.png)

+ 点击New connection弹出界面

  ![image-20220120163514198](C:%5CUsers%5C%E8%93%9D%E6%AD%A3%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20220120163514198.png)

+ 填写内容
  + connection是你创造的这个选项卡的名字，可随意
  + Host填写你的ip或者主机地址
  + Baset填写你的根形如 ，dc=xwy,dc=com。
  + 勾掉Anonymous connection
  + username填写你ldap的管理员用户名
  + password为密码
  + 之后ok即可

> 注意如果报错，一定要检查一下自己填写的字段是否正确，端口有没有通行，selinux安全策略有没有调整模式之类的



#### 总结

+ 下载安装包
+ 解压或不解压
+ 填写字段
+ 登录
+ 总体来说还是非常简单