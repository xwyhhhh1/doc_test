## bknodeman节点管理学习

### 1.安装节点报错

![image-20220216141306892](http://xwyhhhh1.test.upcdn.net/image-20220216141306892.png)

+ 本以为是机器问题，或者是安装agent的机器agent节点没起来，第一次尝试

   + ```shell
      # 查询bknodeman在哪台机器上
      grep -E  "nodeman" /data/install/install.config
      # 登录那台机器，因为我是直接用远程连接工具crt登录，并没有用ssh
      ssh ip
      # 首先是查看日志
      /data/bkce/logs/nodeman/bk_nodeman
      # tail -f *.log 日志中所显示的内容跟平台上的没区别，不用看
      /data/bkcelogs/nodeman
      ```

   + ![](http://xwyhhhh1.test.upcdn.net/image-20220216142707517.png)

   + 看不懂，但是能确定的是，有什么东西没传过去

   + 所以我认为可能是agent端服务没跑起来

   + ```shell
      # 果断跑过去，重启一下
      cd /usr/local/gse/agent/bin/
      ```

   + <img src="http://xwyhhhh1.test.upcdn.net/image-20220216143222929.png" alt="image-20220216143222929" style="zoom: 200%;" />

   + 结果是我想多了，并没有作用，失败

+ 查询问答社区第二次尝试

   + https://bk.tencent.com/s-mart/community/question/5431?type=answer

   + https://bk.tencent.com/s-mart/community/question/5233?type=answer

   + 看了两篇文章，agent的服务为gse，其下还有其他服务，但是管理是中这个没错

   + ```shell
      # 回到中控机，查看状态
      ./bkcli satus gse
      ./bkcli check gse
      ```

   + ![image-20220216143921585](https://xwyhhhh1.test.upcdn.net/image-20220216143921585.png)

   + 检查结果，惨不忍睹，重启之后也是样子

   + ![image-20220216144201414](http://xwyhhhh1.test.upcdn.net/image-20220216144201414.png)

   + 照着文章修改了之后重启运行，是这样子

   + ![image-20220216144440870](http://xwyhhhh1.test.upcdn.net/image-20220216144440870.png)

   + 查看状态都在忙碌，进程pid也在，检查是有点问题。但是服务已经在工作

   + 再试

   + ![image-20220216144646078](http://xwyhhhh1.test.upcdn.net/image-20220216144646078.png)

   + 成功，我只能说好家伙，至于问题原因官方给了一篇文章 。地址：https://systemd-devel.freedesktop.narkive.com/eIQzMZSl/option-to-wait-for-pid-file-to-appear













