## 编译安装apache正式版

#### 1.下载apr、apr-util、pcre

+ 清华园可以搜到或者去官网找(清华源地址：https://mirrors.tuna.tsinghua.edu.cn/)

+ httpd (http://archive.apache.org/dist/httpd/)

   ![image-20220213160655941](http://xwyhhhh1.test.upcdn.net/image-20220213160655941.png)

+ apr (https://mirrors.tuna.tsinghua.edu.cn/apache/apr/)

   ![image-20220213160850254](http://xwyhhhh1.test.upcdn.net/image-20220213160850254.png)

+ apr-util (https://mirrors.tuna.tsinghua.edu.cn/apache/apr/)

   ![image-20220213160957833](http://xwyhhhh1.test.upcdn.net/image-20220213160957833.png)

+ pcre (https://sourceforge.net/projects/pcre/files/pcre/)

   ![image-20220213161125521](http://xwyhhhh1.test.upcdn.net/image-20220213161125521.png)

#### 2. 开始安装

+ 暗转依赖

   + ```shell
      yum install -y gcc gcc-c++ expat-devel
      ```

   + 

+ 首先安装

<img src="http://xwyhhhh1.test.upcdn.net/image-20220213155956154.png" style="zoom:150%;" />

![image-20220213180238073](http://xwyhhhh1.test.upcdn.net/image-20220213180238073.png)