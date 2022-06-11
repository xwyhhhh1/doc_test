## 蜂巢能源蓝鲸PoC

1.前提条件

![image-20220321113920920](https://tva1.sinaimg.cn/large/e6c9d24egy1h0hcq1dm9wj20kq07wt9g.jpg)

2.MAC地址

ec:eb:b8:9d:8e:0c
98:f2:b3:25:65:b4

3.准备蓝鲸企业版软件包

![image-20220321143417171](https://tva1.sinaimg.cn/large/e6c9d24egy1h0hhrzdyb3j20dj03gdft.jpg)

4.在CRT上配置目录和云主机

- CRT上创建目录

![image-20220321143531734](https://tva1.sinaimg.cn/large/e6c9d24egy1h0hht9l1drj20c107m74r.jpg)

- 通过 quick connection 创建5台主机登陆
- 注意：
  - 云主机中是否有TMOUT的变量，如果有，执行命令` unset TMOUT` 取消该变量配置
  - CRT配置长时间如输入时，发空信号保持CRT与主机连接
    ![image-20220321144246341](https://tva1.sinaimg.cn/large/e6c9d24egy1h0hi0sxp4yj20ls0fldhh.jpg)
  - 为了保证多主机连接时，较容易识别，tab名自动获取云主机IP
    ![image-20220321144410550](https://tva1.sinaimg.cn/large/e6c9d24egy1h0hi29dlzgj20ls0fljsu.jpg)

5.上传蓝鲸企业版程序包到bk1主机，通过SecureFX工具：

![image-20220321144512491](https://tva1.sinaimg.cn/large/e6c9d24egy1h0hi3f5ja2j21400n578n.jpg)

```shell
# bk1 主机操作
cd ~
mkdir bk-fcny
cd bk-fcny

#上传蓝鲸企业版程序包4个,rz 也可以，SecureFX也可以，建议后者。
rz *.gz 
```

