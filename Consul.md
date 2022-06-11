## Consul

### 一、安装部署

#### 1.下载consul

```shell
#选择去到官网下载仓库还是文件
#此为官网地址
https://www.consul.io/downloads
#此为官网配置，由此安装consul
yum install -y yum-utils
yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
yum -y install consul
#或是去官网下载如下图所示的压缩包
#因为下载之后为zip后缀的压缩格式文件，因此请使用unzip解压
unzip xxxx.zip -d /usr/local/src
```

![image-20220404165211768](http://xwyhhhh1.test.upcdn.net/image-20220404165211768.png)

#### 2.安装部署

```shell
#压缩文件安装，请复制解压后的consul二进制可执行文件，到/usr/local/bin下
#压缩文件安装，解压后只会有一个名为consul的二进制文件
cp /usr/local/src/consul /usr/local/bin
#检查是否能够找到命令
which consul
#检查成功请尝试使用命令检查版本
consul version
#yum安装请在安装过后，尝试查看版本，确认已经安转成功
consul version
```

### 二、集群化Cosul

#### 1.启动concul，加入集群

```shell
#启动consul，需要使用consul命令，请确认consul命令能被找到
#选择一台主机作为集群主机，担当lender角色
consul agent -server -bind=192.168.0.250 -data-dir=/etc/consul.d -booststarp-expect 3 -client=0.0.0.0 -ui &
#启动从机
consul agrnt -server -bind=192.168.0.251 -data-dir=/etc/consul.d -client=0.0.0.0 -ui &
consul agrnt -server -bind=192.168.0.252 -data-dir=/etc/consul.d -client=0.0.0.0 -ui &
#各节点启动成功，但是需要集群化，否则consul没有lender角色，运行会一直报错
#因此要加入集群，以下操作请分别在其它从机上进行一遍，此为加入集群操作
consul join 192.168.0.250
```

####2.查询结果

```shell
#查看consul集群成员
consul members
#查看lender角色是谁
consul operatop raft -list-peers
#以下为标准输出结果，请查看lender为哪台主机
Node   ID                  Address             State     Voter
test3  192.168.0.251:8300  192.168.0.251:8300  follower  true
ceshi  192.168.0.250:8300  192.168.0.250:8300  leader    true
test2  192.168.0.252:8300  192.168.0.252:8300  follower  true
```

### 三、额外补充

#### 1.启动参数解释

```shell
-server:此为以agent server模式启动consul，在不指定此参数时，默认以client参数为替换
-bind:指定目标主机，此为本主机ip，填其它主机会报错哟
-data-dir=:指定consul相关数据文件目录，默认是consul.d，consul会将集群信息打印到数据存放目录
-bootstarp-rxprct:指定集群最少sever数，少于此数consul即停止运行
-client= -ui:0.0.0.0允许任何主机访问ui，也就是web界面，不加此参数只加-ui，则只能在主机上访问
-ui:允许访问web界面
-node:此参数只能节点名称，默认为主机名，但是可以通过 -node 名称，来修改名称
-dc:指定consul agent加入到哪一个数据节点，默认为dc1，同样可以通过 -dc 名称，来修改数据中心
-config-dir=:指定配置文件存放地
consul keygen:此为现实consul公钥命令
```

#### 2.其它启动模式

```shell
#进入开发模式，此模式下不适合生产模式，此模式无法持久化数据，也就是写入磁盘，一般用做单机consul或者其它
consul -dev
#以client启动consul
consul agrnt -bind=192.168.0.251 -data-dir=/etc/consul.d -client=0.0.0.0 -ui


#一般来说，一个consul集群，最少要有是三个server，两个client，这点请记住！！！
```

#### 3.通过配置文件添加配置

```shell
#请将文件命名为consfig.json，请将她存放在consul.d下，请使用-config-dir指定配置文件存放地址
#请以json格式撰写文件
#请选择一台主机作为consul lender执行consul keygen命令复制加密值
#请参考以下格式
{undefined

"bootstrap_expect": 3,

"client_addr": "0.0.0.0",

"datacenter": "walmart-datacenter1",

"data_dir": "/data/consul",

"domain": "consul",

"enable_script_checks": true,

"dns_config": {undefined

"enable_truncate": true,

"only_passing": true

},

"enable_syslog": true,

"encrypt": "dwa+3N9rgVYUsthbk5w/vds/9x3ambw0WgaFculhXmY=", #请将加密值修改为自己的

"leave_on_terminate": true,

"log_level": "INFO",

"rejoin_after_leave": true,

"server": true,

"start_join": [

"192.168.10.136",

"192.168.10.128",

"192.168.10.16"

],

"ui": true

}
```

