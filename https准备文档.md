```bash
10.1.1.196 iam,ssm,usermgr,gse,license,redis,consul,mysql,lesscode
10.1.1.173 nginx,consul,mongodb,rabbitmq,appo
10.1.1.232 paas,cmdb,job,zk(config),appt,consul,nodeman(nodeman)
```

```SHELL
10.1.1.173 paas.zgdx.com cmdb.zgdx.com job.zgdx.com  jobapi.zgdx.com
10.1.1.232 nodeman.zgdx.com



10.0.0.2 paas.zgdx.com cmdb.zgdx.com job.zgdx.com jobapi.zgdx.com lesscode.zgdx.com
10.0.0.3 nodeman.zgdx.com
```







```bash
BK_HTTP_SCHEMA=https
# 访问PaaS平台的域名
BK_PAAS_PUBLIC_URL="https://paas.zgdx.com"
BK_PAAS_PUBLIC_ADDR="paas.zgdx.com:443"
BK_PAAS_PRIVATE_ADDR="paas.service.consul:80"
BK_PAAS_PRIVATE_URL="http://paas.service.consul"
# 访问CMDB的域名
BK_CMDB_PUBLIC_ADDR="cmdb.zgdx.com:443"
BK_CMDB_PUBLIC_URL="https://cmdb.zgdx.com"
# 访问Job平台的域名
BK_JOB_PUBLIC_ADDR="job.zgdx.com:443"
BK_JOB_PUBLIC_URL="https://job.zgdx.com"
BK_JOB_API_PUBLIC_ADDR="jobapi.zgdx.com:443"
BK_JOB_API_PUBLIC_URL="https://jobapi.zgdx.com"
# 访问节点管理下载插件包的URL前缀
BK_NODEMAN_PUBLIC_DOWNLOAD_URL="https://nodeman.zgdx.com:443"
# lesscode 域名
BK_LESSCODE_PUBLIC_ADDR='lesscode.zgdx.com:443'
BK_LESSCODE_PUBLIC_URL='https://lesscode.zgdx.com:443'
```