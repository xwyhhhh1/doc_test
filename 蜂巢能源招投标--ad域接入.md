## 蜂巢能源招投标——ad域控接入

#### 系统实施接入部分——ad域控接入

##### ad域控接入

##### 对接ad域控

进入用户管理SaaS，选中用户目录，点击新增目录，请以admin身份进入，否则可能靠能造成权限不足等问题，选择ad域项，点击确定，进入配置界面

![image-20220416104615583](http://xwyhhhh1.test.upcdn.net/image-20220416104615583.png)

![image-20220416104736615](http://xwyhhhh1.test.upcdn.net/image-20220416104736615.png)

首先是基本设置页面，目录名名字建议为接入的ad域的名称，域名为，接入ad域后，ad域用户登录蓝鲸需要指定的域名，蓝鲸支持对于目录的启用或不启用选项，可由右侧的勾选项确认要否启用该对接目录，点击下一步进入连接设置。

![image-20220416104856616](http://xwyhhhh1.test.upcdn.net/image-20220416104856616.png)

其次是连接设置，填写ad域的基本信息，可选择加密方式。填写基本信息确认无误后，可进行测试连接，以此判断是否联通

![image-20220416105213399](http://xwyhhhh1.test.upcdn.net/image-20220416105213399.png)

最后为字段配置，选择要拉取的字段，建议整个域都拉取，对于扩展字段，此为接入蓝鲸用户，对用户基础信息显示的部分，最后的用户名字段项，为要拉取时根据何关键字进行。基本主要配置拉取字段的信息，填写用户基础字段信息，其他字段信息可以保持默认即可。

![image-20220416105533208](http://xwyhhhh1.test.upcdn.net/image-20220416105533208.png)

![image-20220416105646836](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20220416105646836.png)

![image-20220416105813069](http://xwyhhhh1.test.upcdn.net/image-20220416105813069.png)

验证，查看同步是否成功是否拉取成功

![image-20201110174344369](http://xwyhhhh1.test.upcdn.net/image-20201110174344369.png)

尝试登录，登录规则为“用户名@域名”

![image-20220416110435480](http://xwyhhhh1.test.upcdn.net/image-20220416110435480.png)