## 国泰君安项目实验

### 一、windows_server_2012_R2

#### 1.采用镜像

##### 1.1windows_server_2012_R2_64位镜像

![image-20220506164628242](http://xwyhhhh1.test.upcdn.net/image-20220506164628242.png)

#### 2.基础环境

##### 2.1云蓝鲸测试环境

##### 2.2版本企业版V3.0.1

#### 3.采用的秘钥

![image-20220506204144203](http://xwyhhhh1.test.upcdn.net/image-20220506204144203.png)

#### 4.测试使用用户

##### 4.1系统管理员用户admin

#### 5.设置

##### 5.1添加规则限制139、445端口

![image-20220506204340569](http://xwyhhhh1.test.upcdn.net/image-20220506204340569.png)

##### 5.2netbios配置

![image-20220507163039830](http://xwyhhhh1.test.upcdn.net/image-20220507163039830.png)

#### 6.测试

##### 6.1部署成功

![image-20220506204943218](http://xwyhhhh1.test.upcdn.net/image-20220506204943218.png)

##### 6.2测试分发文件

![image-20220506224105869](http://xwyhhhh1.test.upcdn.net/image-20220506224105869.png)

![image-20220506223417860](http://xwyhhhh1.test.upcdn.net/image-20220506223417860.png)

##### 6.3测试执行命令

![image-20220506223829062](http://xwyhhhh1.test.upcdn.net/image-20220506223829062.png)

### 二、windows_server_2008_R2

#### 1.采用镜像

##### 1.1windows_server_2008_R2_64位镜像



![image-20220507095901977](http://xwyhhhh1.test.upcdn.net/image-20220507095901977.png)

#### 2.基础环境

##### 2.1云蓝鲸测试环境

##### 2.2版本企业版V3.0.1

#### 3.测试使用用户

##### 4.1系统管理员用户admin

#### 4.设置

##### 4.1添加规则限制139、445端口

![image-20220507114012463](http://xwyhhhh1.test.upcdn.net/image-20220507114012463.png)

##### 4.2netbios设置

![](http://xwyhhhh1.test.upcdn.net/image-20220507163039830.png)

##### 4.3高级网络共享设置

![image-20220507103603099](http://xwyhhhh1.test.upcdn.net/image-20220507103603099.png)

#### 5.测试

##### 5.1部署成功

![image-20220507104537436](http://xwyhhhh1.test.upcdn.net/image-20220507104537436.png)

##### 5.2节点状态和插件状态

![image-20220507104559889](http://xwyhhhh1.test.upcdn.net/image-20220507104559889.png)

![image-20220507104626153](http://xwyhhhh1.test.upcdn.net/image-20220507104626153.png)

##### 5.3测试分发文件

![image-20220507104801175](http://xwyhhhh1.test.upcdn.net/image-20220507104801175.png)



![image-20220507104833881](http://xwyhhhh1.test.upcdn.net/image-20220507104833881.png)

##### 5.4测试执行命令

![image-20220507104941958](http://xwyhhhh1.test.upcdn.net/image-20220507104941958.png)

### 三、windows_server_2016

#### 1.采用镜像

##### 1.1windows_server_2008_R2_64位镜像



![image-20220507105200896](http://xwyhhhh1.test.upcdn.net/image-20220507105200896.png)

#### 2.基础环境

##### 2.1云蓝鲸测试环境

##### 2.2版本企业版V3.0.1

#### 3.测试使用用户

##### 4.1系统管理员用户admin

#### 4.设置

##### 4.1添加规则限制139、445端口

![image-20220507111259604](http://xwyhhhh1.test.upcdn.net/image-20220507111259604.png)

##### 4.2netbios配置

![image-20220507163039830](http://xwyhhhh1.test.upcdn.net/image-20220507163039830.png)

##### 4.3高级网络共享设置

![image-20220507111040859](http://xwyhhhh1.test.upcdn.net/image-20220507111040859.png)

#### 5.测试

##### 5.1部署成功

![image-20220507112141570](http://xwyhhhh1.test.upcdn.net/image-20220507112141570.png)

##### 5.2节点状态

![image-20220507112730559](http://xwyhhhh1.test.upcdn.net/image-20220507112730559.png)

##### 5.3测试分发文件

![image-20220507112607634](http://xwyhhhh1.test.upcdn.net/image-20220507112607634.png)

![image-20220507115028244](http://xwyhhhh1.test.upcdn.net/image-20220507115028244.png)

![image-20220507115453846](http://xwyhhhh1.test.upcdn.net/image-20220507115453846.png)

##### 5.4测试执行命令

![image-20220507112843377](http://xwyhhhh1.test.upcdn.net/image-20220507112843377.png)



### 四、结论

#### 1.端口禁止

在设置入站规则禁止445端口和139端口，手动安装agent能够成功，执行命令和分发文件没有问题，但是插件会有entire，不显示状态并且通过蓝鲸平台对插件进行操作会出现问题

#### 2.功能

如上所测，测试分发文件没有问题以及执行命令也没有问题



