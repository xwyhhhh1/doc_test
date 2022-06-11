## 蓝鲸升级	v1.v2.V3--2022-2-18

#### 1.usermgr升级第一种方案--验证成功（废弃）

+ ```shell
   # 上传usermanger包
   rz
   # 备份之前的安装包
   mkdir /root/bakup
   mv /data/src/usermgr_ce-2.2.6-b3-bkofficial.tar.gz /root/backup
   mv $PACKAGES /data/src/
   source /data/install/utils.fc
   ./bkcli stop usermgr
   ./bkcli install usermgr或./bkcli upgrade usermgr 
   ./bkcli start usermgr
   ./bkcli install saas-o bk_user_manage
   ```

   #### 2.usermgr升级第二种方案--验证成功（测试环境，起码命令没问题）（第三次修改未测）
   
+ ```shell
   # 上传usermanger包
   rz
   # 备份之前的安装包
   mkdir /root/bakup
   mv /data/src/usermgr_ce-2.2.6-b3-bkofficial.tar.gz /root/backup
   mv $PACKAGES /data/src/
   # 直接安装覆盖
   cd /data/install/
   # 同步usermgr
   ./bkcli sync usermgr
   # 更新usermgr
   ./bkcli upgrade usermgr
   # 初始化usermgr安装后初始化模块，如数据库、权限模型等。
   ./bkcli initdate usermgr
   # 重新渲染usermgr
   ./bkcli render usermgr
   # 重新启动usermgr
   ./bkcli restart usermgr
   ```

