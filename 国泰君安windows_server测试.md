## 国泰君安项目实验

### 一、windows_server_2012_R2_64版本（对防火墙不进行操作）

#### 1.采用镜像

##### 1.1windows_server_2012_R2_64位镜像

![image-20220506164628242](http://xwyhhhh1.test.upcdn.net/image-20220506164628242.png)

#### 2.基础环境

##### 2.1上海本地蓝鲸测试环境

##### 2.2版本社区版V6.0.5

#### 3.采用的秘钥

![image-20220506164736221](http://xwyhhhh1.test.upcdn.net/image-20220506164736221.png)

#### 4.测试使用用户

##### 4.1系统管理员用户admin

#### 5.设置

##### 5.1防火墙状态为开启

![image-20220506172450359](http://xwyhhhh1.test.upcdn.net/image-20220506172450359.png)

##### 5.2开启netbios

![image-20220506182913391](http://xwyhhhh1.test.upcdn.net/image-20220506182913391.png)

#### 6.测试

##### 6.1部署成功

![image-20220506183253602](http://xwyhhhh1.test.upcdn.net/image-20220506183253602.png)

### 一、windows_server_2012_R2_64版本（对防火墙采用禁止端口规则）

#### 1.采用镜像

##### 1.1windows_server_2012_R2_64位镜像

![image-20220506164628242](http://xwyhhhh1.test.upcdn.net/image-20220506164628242.png)

#### 2.基础环境

##### 2.1上海本地蓝鲸测试环境

##### 2.2版本社区版V6.0.5

#### 3.采用的秘钥

![image-20220506204144203](http://xwyhhhh1.test.upcdn.net/image-20220506204144203.png)

#### 4.测试使用用户

##### 4.1系统管理员用户admin

#### 5.设置

##### 5.1添加规则限制139、445端口

![image-20220506204340569](http://xwyhhhh1.test.upcdn.net/image-20220506204340569.png)

##### 5.2开启netbios

![image-20220506182913391](http://xwyhhhh1.test.upcdn.net/image-20220506182913391.png)

#### 6.测试

##### 6.1部署成功

![image-20220506204943218](http://xwyhhhh1.test.upcdn.net/image-20220506204943218.png)

##### 6.2测试分发文件

![image-20220506205125519](http://xwyhhhh1.test.upcdn.net/image-20220506205125519.png)

##### 6.3测试执行命令

![image-20220506205215910](http://xwyhhhh1.test.upcdn.net/image-20220506205215910.png)



![image-20220506223417860](http://xwyhhhh1.test.upcdn.net/image-20220506223417860.png)

![image-20220506223829062](http://xwyhhhh1.test.upcdn.net/image-20220506223829062.png)



![image-20220506224105869](http://xwyhhhh1.test.upcdn.net/image-20220506224105869.png)
