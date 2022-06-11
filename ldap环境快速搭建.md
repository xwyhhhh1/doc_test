## ldap环境快速搭建

#### 1.安装相关软件包

```shell
yum install -y openldap openldap-clients openldap-servers
```

#### 2.复制一个默认配置到指定目录下,并授权，且启动服务

```shell
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
#递归赋予权限
chown -R ldap. /var/lib/ldap/DB_CONFIG
systemctl start slapd #启动进程
systemctl enable slapd #设置开机自启
```

#### 3.修改openLDAP配置

```shell
slappasswd -s 123456 #生成密码
SSHA}Dhs2hPiBZivZgJMZB1ZGv9HZR1loC1iU #加密之后的密码，每个人的不一样，记得保存
vim changepwd.ldif #以下为配置文件
dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: SSHA}Dhs2hPiBZivZgJMZB1ZGv9HZR1loC1iU #修改成自己生成的

ldapadd -Y EXTERNAL -H ldapi:/// -f changepwd.ldif #导入ldap服务器
#建议直接输出特殊变量验证结果
echo $?
```

#### 4.导入Schema

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

#### 5.增加管理域

```shell
vim changedomain.ldif

dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth" read by dn.base="cn=admin,dc=yaobili,dc=com" read by * none #可以进行修改，cn此处是管理员账号 dc dc

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=yaobili,dc=com #同样如此

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=admin,dc=yaobili,dc=com #同样如此

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}LSgYPTUW4zjGtIVtuZ8cRUqqFRv1tWpE #修改加密密码

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by dn="cn=admin,dc=yaobili,dc=com" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=admin,dc=yaobili,dc=com" write by * read #同样如此

ldapmodify -Y EXTERNAL -H ldapi:/// -f changedomain.ldif #导入ldap服务器
```

### 6.增加组织用户

```shell
vim base.ldif
dn: dc=yaobili,dc=com
objectClass: top
objectClass: dcObject
objectClass: organization
o: Yaobili Company
dc: yaobili

dn: cn=admin,dc=yaobili,dc=com
objectClass: organizationalRole
cn: admin

dn: ou=People,dc=yaobili,dc=com
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=yaobili,dc=com
objectClass: organizationalRole
cn: Group
ldapadd -x -D cn=admin,dc=yaobili,dc=com -W -f base.ldif #导入
#会需要输入密码，密码为自己生成的那个
```



#### 7.安装web端管理工具phpldapadmin，修改配置（可做可不做）

```shell
yum -y install httpd #安装httpd服务作为网络服务支撑
yum -y install phpldapadmin #安装工具
vim /etc/httpd/conf.d/phpldapadmin.conf #修改访问配置，允许任何主机访问

  <IfModule mod_authz_core.c>
    # Apache 2.4
    Require all granted #修改成这个样子
  </IfModule>

vim /etc/phpldapadmin/config.php +398
$servers->setValue('login','attr','cn');

vim /etc/phpldapadmin/config.php +460
$servers->setValue('login','anon_bind',false);

vim /etc/phpldapadmin/config.php +519
$servers->setValue('unique','attrs',array('mail','uid','uidNumber','cn','sn'));

systemctl start httpd #启动
systemctl enable httpd #开机自启
```

#### 8.登录网站

http://ip/phpldapadmin





#### 9.额外补充

**添加mamberOf属性**

> 这个启用memberOf属性的方法可能有并不是那么靠谱，建议寻找其他的开启方法

```shell
vim add-memberof.ldif
dn: cn=module{0},cn=config
cn: modulle{0}
objectClass: olcModuleList
objectclass: top
olcModuleload: memberof.la
olcModulePath: /usr/lib64/openldap

dn: olcOverlay={0}memberof,olcDatabase={2}hdb,cn=config
objectClass: olcConfig
objectClass: olcMemberOf
objectClass: olcOverlayConfig
objectClass: top
olcOverlay: memberof
olcMemberOfDangling: ignore
olcMemberOfRefInt: TRUE
olcMemberOfGroupOC: groupOfUniqueNames
olcMemberOfMemberAD: uniqueMember
olcMemberOfMemberOfAD: memberOf

vim refint1.ldif
dn: cn=module{0},cn=config
add: olcmoduleload
olcmoduleload: refint

vim refint2.ldif
dn: olcOverlay=refint,olcDatabase={2}hdb,cn=config
objectClass: olcConfig
objectClass: olcOverlayConfig
objectClass: olcRefintConfig
objectClass: top
olcOverlay: refint
olcRefintAttribute: memberof uniqueMember  manager owner

ldapadd -Q -Y EXTERNAL -H ldapi:/// -f add-memberof.ldif
ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f refint1.ldif
ldapadd -Q -Y EXTERNAL -H ldapi:/// -f refint2.ldif
#按顺序导入
```

+ 基准 dc=yaobili,dc=com 是该树的根节点，其下有一个管理域 cn=admin,dc=yaobili,dc=com 和两个组织单元 ou=People,dc=yaobili,dc=com 及 ou=Group,dc=yaobili,dc=com

*参考文章：https://blog.csdn.net/weixin_41004350/article/details/89521170*