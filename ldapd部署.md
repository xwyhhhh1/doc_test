# ldap部署

> ##### **准备事项**

+ 关闭防火墙，或者通行389和80端口

  ```shell
  firewall-cmd --add-port=389/tcp --permanent#通行389端口
  firewall-cmd --add-port=80/tcp --permanent#通行80端口
  systemctl reload firewalld#重载防火墙
  #通行端口
  ```

  ```shell
  systemctl stop firewalld#此为关闭防火墙
  ```

+ 改变selinux安全策略

  ```shell
  setenforce 0#改为强制模式
  ```

+ 安装apache服务

  ```SHELL
  yum -y install httpd
  ```

> **问题**：如果安装软件包失败，nothing to do 表示你已经装过了；提示没有软件包的话 not package，建议下载阿里源或者挂载本地光盘，这里建议直接下载阿里的epel源

```shell
wget http://mirrors.aliyun.com/repo/epel-7.repo
```

这里还有问题就是，wget命令提示没有的话就用icurl 

```shell
icurl -O http://mirrors.aliyun.com/repo/epel-7.repo
```

#### 安装openldap的包，拷贝数据文件赋予权限

```shel
yum install -y openldap openldap-clients openldap-servers
```

![image-20220118124629505](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118124629505.png)

##### 查看没有问题

![image-20220118124732903](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118124732903.png)

#### 启动服务，设置开机自启，查看服务状态，也ok

![image-20220118124904750](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118124904750.png)

#### 生成密码

![image-20220118132421168](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118132421168.png)

```shell
vim changepwd.ldif#创建初始配置文件
```

> **olcRootpw**更改为我们自己生成的加密密钥

![image-20220118133003332](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118133003332.png)

#### 导入文件

![image-20220118133125578](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118133125578.png)

#### 导入schema

```shell
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/collective.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/corba.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/duaconf.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/dyngroup.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/java.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/misc.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/openldap.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/pmi.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/ppolicy.ldif
```

![image-20220118134008128](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118134008128.png)

#### 编辑域文件
```she
vim changedomain.ldif
```

> **olRootpw**同样需要更改，以及cn，dc,更改成自己的也行，如果怕出错就不改

![image-20220118134229654](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118134229654.png)

#### 导入文件

```shell
ldapmodify -Y EXTERNAL -H ldapi:/// -f changedomain.ldif
```

![image-20220118135536831](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118135536831.png)

最后的一行报错是我多打了一遍

#### 创建用户

​	

![image-20220118140301164](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118140301164.png)

#### 导入

```shell
ldapadd -x -D cn=admin,dc=yaobili,dc=com -W -f base.ldif
```

![image-20220118140350803](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118140350803.png)

##### 配置phpldapadmin

```shell
vim /etc/httpd/conf.d/phpldapadmin.conf
```

![image-20220118141921236](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118141921236.png) 

```shell
vim /etc/phpldapadmin/config.php
```

+ 398行

![image-20220118142132034](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118142132034.png)

+ 460行

![image-20220118142307414](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118142307414.png)

+ 519行

![image-20220118142432148](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118142432148.png)

#### 启动apache服务

![image-20220118142527746](C:\Users\蓝正\AppData\Roaming\Typora\typora-user-images\image-20220118142527746.png)





### 总结：

+ 完成准备工作

+ 安装软件包
+ 添加配置文件
+ 添加用户
+ 安装phpldapdadmin
+ 修改配置文件