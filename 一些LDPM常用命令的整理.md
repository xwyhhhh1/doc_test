# 一些LDPM常用命令的整理

### ldapadd命令: 通过ldif格式的文件，添加目录树条目

+ 参数介绍
  + -x 进行简单认证
  + -D 用来绑定服务器DN
  + -H 目录服务的地址
  + -f  使用ldif进行条目添加的文件
  + -w 绑定DN密码
  + -Y 使用安全的LDAP连接

+ 用法举例
  ```she
  ldapadd -x -D"cn=xxx,dc=xxx,dc=xxx" -w (password)  -f xxxx.ldif 
  #指定dn将xxx.ldif文件中的操作(一般是添加用户或组操作)导到"cn=xxx,dc=xxx,dc=xxx"中
  ```

  ```shell
  ldapadd -Y EXTERNAL -H ldapi:///ip或者主机名 -w -f xxx.ldif
  #这是用来对指定的Openldap服务器做添加配置文件操作的
  ```

### ldapsearch命令

+ 参数介绍

  + -x 进行简单认证
  + -D 用来绑定服务器DN
  + -W 绑定DN的密码
  + -b  指定要查询的根节点
  + -H  制定要查询的服务器

+ 用法举例

  ```shell
  ldapserach -x -D "cn=xxx,dc=xxx,dc=xxx" -W (password) -b "dc=xxx,dc=xxx"
  # 使用简单认证，用 "cn=root,dc=starxing,dc=com" 进行绑定，查询的是"dc=starxing,dc=com"根
  ```

  ```shell
  ldapdelete -x -D 'cn=root,dc=it,dc=com' -w secert 'uid=zyx,dc=it,dc=com'
  #绑定cn=root,dc=it,dc=com管理员用户，删除'uid=zyx,dc=it,dc=com'
  ```

### ldappasswd命令

+ 参数介绍
  +   -x  进行简单认证
  +   -D  用来绑定服务器的DN
  +   -w  绑定DN的密码
  +   -S  提示的输入密码
  +   -s pass 把密码设置为pass
  +   -a pass 设置old passwd为pass
  +   -A  提示的设置old passwd
  +   -H  是指要绑定的服务器
  +   -I  使用sasl会话方式

+ 用法举例

  ```shell
  ldappasswd -x -D 'cm=root,dc=it,dc=com' -w secret 'uid=zyx,dc=it,dc=com' -S
  #更改密码，如果之前无密码，就会生成一个密码
  ```

### ldapmodify命令

+ 参数介绍

  + -H ldapurl，格式为ldap://机器名或者IP:端口号，不能与-h和-p同时使用
  + -h LDAP服务器IP或者可解析的hostname，与-p可结合使用，不能与-H同时使用
  + -p LDAP服务器端口号，与-h可结合使用，不能与-H同时使用
  + -x 使用简单认证方式
  + -D 所绑定的服务器的DN
  + -w 绑定DN的密码，与-W二者选一
  + -W 不输入密码，会交互式的提示用户输入密码，与-w二者选一
  + -c  出错后忽略当前错误继续执行，缺省情况下遇到错误即终止
  + -n 模拟操作但并不实际执行，用于验证，常与-v一同使用进行问题定位
  + -v  显示详细信息
  + -d 显示debug信息，可设定级别
  + -e 设置客户端证书

+ 用法举例（modify操作一般都是通过ldif文件通过在文件内编写Openldap参数来进行修改）

  ```shell
  ldapmodify -a -H ldap://192.168.31.242:389 -D "cn=xxx,dc=xxx,dc=xxx" -w admin -f xxx.ldif
  #将提前编写好的ldif文件导入以完成操作
  ```

### 总结 ：ldap许多操作需要依赖在.ldif文件中的编写参数，于是更应该系统的学习参数的编写，命令更多的只是导入和查询的工具。