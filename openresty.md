## openresty

一、安装部署（源码编译）

```shell
yum -y install perl-devel readline-devel pcre-devel openssl-devel gcc gcc-c++
cd /usr/local/src/
wegt https://openresty.org/download/openresty-1.19.9.1.tar.gz
tar -xf openresty-1.19.9.1.tar.gz
cd openresty-1.19.9.1
./configure --prefix=/usr/local/openresty
echo &?
make && make install
cd /usr/local/openresty/nginx/sbin
./nginx -V
./nginx
```

二、yum安装

```shell
wget https://openresty.org/package/centos/openresty.repo
mv openresty.repo /etc/yum.repo.d/
#安装openresty
yum -y install openresty
#升级openresty,此步操作系已安装openresty操作基础下升级现有版本openresty，仅仅安装此步操作可忽略
yum update openresty
```

三、升级openssl（源码部署openresty升级openssl）

```shell
cd /usr/local/src
yum install -y  zlib zlib-devel gcc gcc-c++ perl
wget https://github.com/openssl/openssl/archive/refs/tags/OpenSSL_1_1_1o.tar.gz
tar -xf OpenSSL_1_1_1o.tar.gz
cd openssl-OpenSSL_1_1_1o
./config --prefix=/usr/local/openssl
echo $?
make && make install
#进行访问nginx测试，观察升级过程中是否会宕掉服务
export a=0 && while :; do let a=$a+1 ; echo $a ; curl -I 192.168.0.196 ; sleep 1 ; done

#新开一个终端，进入之前的openresty目录下
cd /usr/local/src/openresty-1.19.9.1
./configure --prefix=/usr/local/openresty --with=openssl=/usr/local/openssl
echo $?
make
cd /build/nginx/sbin
mv /usr/local/openresty/nginx/sbin/nginx /usr/local/openresty/nginx/sbin/nginx.ori
cp ./nginx /usr/local/openresty/nginx/sbin/
cd /usr/local/openresty/nginx/sbin/
./nginx -s reload
./nginx -V #观察输出，可以看到openssl版本已经变为1.1.1o并且观察之前循环访问测试，发现服务并未中断，至此，已完成openresty升级使用的openssl服务的升级，这套升级操作可用于正式的环境
```

