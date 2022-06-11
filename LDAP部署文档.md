## LDAP部署文档

#### 一.openldap安装

```shell
## 安装包
yum install -y openldap openldap-clients openldap-servers migrationtools

## 设置OpenLDAP管理密码
slappasswd
输入：uojPQqproks86xcq
{SSHA}GYumdfOdnNyVnueiLy+wxK3E0L2zooaD
{SSHA}7cBWtleNj/IhdjuBuYSlI6tej8mOtn7C

## 修改根DN与添加密码
vim /etc/openldap/slapd.d/cn\=config/olcDatabase\=\{2\}hdb.ldif
olcSuffix: dc=example,dc=com
olcRootDN: cn=admin,dc=example,dc=com
olcRootPW: {SSHA}GYumdfOdnNyVnueiLy+wxK3E0L2zooaD

## 修改验证
vim /etc/openldap/slapd.d/cn\=config/olcDatabase\=\{1\}monitor.ldif
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=extern
al,cn=auth" read by dn.base="cn=admin,dc=example,dc=com" read by * none

## 配置DB数据库
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
chown -R ldap.ldap /var/lib/ldap

## 验证
slaptest -u                 -> config file testing succeeded 验证成功，否则失败。

## 启动
systemctl start slapd
systemctl enable slapd

## 开启日志
mkdir -p /var/log/slapd
chown ldap:ldap /var/log/slapd/
touch /var/log/slapd/slapd.log
chown ldap . /var/log/slapd/slapd.log
echo "local4.* /var/log/slapd/slapd.log" >> /etc/rsyslog.conf
systemctl restart rsyslog
```

#### 

#### 二.验证

```shell
ldapsearch -x -b '' -s base'(objectclass=*)'                -> 执行看是否报错
```



#### 三.创建管理员账号

```shell
## 编辑文件
vim admin.ldif
dn: dc=example,dc=com
objectclass: dcObject
objectclass: organization
o: example.com
dc: example

dn: cn=admin,dc=example,dc=com
objectclass: organizationalRole
cn: admin

## 导入数据库
ldapadd -x -D "cn=admin,dc=example,dc=com" -W -f admin.ldif

## 验证
ldapsearch -x -b 'dc=example,dc=com' '(objectClass=*)'
```



#### 四.导入基本schema

```shell
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif 
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
 
Schema由 对象类（ObjectClasses） 、属性类型（ Attribute Types） 、属性语法（Syntaxes）、匹配规则（Matching Rules）构成
```



#### 五.重新搭建openldap的方法

```shell
rm /var/lib/ldap/DB_CONFIG
cp -f /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
chown ldap. /var/lib/ldap/DB_CONFIG
```



#### 六.添加组织

```shell
## vim base.ldif
dn: ou=Ops,dc=example,dc=com
objectclass: top
objectclass: organizationalUnit
ou: Ops

dn: ou=Dev,dc=example,dc=com
objectclass: top
objectclass: organizationalUnit
ou: Dev

dn: ou=QA,dc=example,dc=com
objectclass: top
objectclass: organizationalUnit
ou: Manager

dn: ou=PM,dc=example,dc=com
objectclass: top
objectclass: organizationalUnit
ou: PM

## 导入
ldapadd -x -D cn=Manager,dc=example,dc=com -w uojPQqproks86xcq -f base.ldif
```



#### 七.添加用户

```shell
dn: uid=shenjieqiong,ou=QA,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
cn: shenjieqiong
sn: shenjieqiong
displayName: 沈洁琼
employeeType: 1
givenName: shenjieqiong
mail: shenjieqiong@examplego.com
ou: QA
uid: shenjieqiong
userPassword: Z2VycDEyMw==

dn: uid=daijie,ou=QA,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
cn: daijie
sn: daijie
displayName: 戴杰
employeeType: 1
givenName: daijie
mail: daijie@examplego.com
ou: QA
uid: daijie
userPassword: Z2VycDEyMw==

dn: uid=gaojie,ou=QA,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
cn: gaojie
sn: gaojie
displayName: 高洁
employeeType: 1
givenName: gaojie
mail: gaojie@examplego.com
ou: QA
uid: gaojie
userPassword: Z2VycDEyMw==
```



#### 错误1

报错：additional info: modify/add: xxx: no equality matching rule

解决办法：将对应属性的add改为replace



#### 参考文档

https://www.jianshu.com/p/34dc6412de30

https://blog.csdn.net/rockstics/article/details/108239475

https://blog.csdn.net/weixin_43423965/article/details/105215588

https://www.cnblogs.com/quliuliu2013/p/11303038.html			-> 使用说明

https://wiki.shileizcc.com/confluence/pages/viewpage.action?pageId=39223494

https://wiki.shileizcc.com/confluence/display/openldap/OpenLDAP+N-Way+Multi-Master -> 使用详细说明