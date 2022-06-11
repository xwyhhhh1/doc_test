## Python--PIP源使用-----linux操作系统

### 1.linux自带python环境

#### 1.1检查自己python版本和pip是否安装

```shell
python --version #查看当前python版本环境，另外，python最新版以及2.7.9以上的版本都自带pip
pip --version #通过此命令查看pip版本，如果提示命令不存在没有pip,若无则看下一步骤1.2
```

#### 1.2安装pip

```shell
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py instead.#若无pip则可以通过下载get-pip.py文件来构建pip
python get-pip.py #是什么版本的要用什么版本的文件，当然也可以下载我这个执行，会有报错提示ERROR: This script does not work on Python 2.7 The minimum supported Python version is 3.7. Please use https://bootstrap.pypa.io/pip/2.7/get-pip.py instead.这个时候下载他给你的网址即可

#中间可能会报错，错误为：ReadTimeoutError: HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Read timed out. 这种情况不用担心，只是下载超时问题，多尝试几次即可

```

### 2.更换pip源

#### 2.1必要性

+ pip为python软件包管理工具，非常便利，但是他默认下载软件包的地址为 https://pip.pypa.io/，因为国内网络情况的原因，下载软件包会异常麻烦，time oout的次数比较多，因此更换pip源很重要

#### 2.2更换pip源

```shell
cd ~ #要使用root用户
mkdir .pip #在root用户下创建一个隐藏的pip目录
vim .pip/pip.conf #创建一个pip配置文件
[global] #此为全局设置
index-url = https://mirrors.aliyun.com/pypi/simple #此为pip源地址
```

#### 2.3其它国内源地址

```shell
  清华：https://pypi.tuna.tsinghua.edu.cn/simple
  中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
  华中理工大学：http://pypi.hustunique.com/
  山东理工大学：http://pypi.sdutlinux.org/
  豆瓣：http://pypi.douban.com/simple/
  # 个人建议清华或者上面我使用的阿里较好
```

#### 2.4临时使用其它pip地址

```shell
pip install 包命 -i 国内源地址
```



*pip搭建参考文章：*https://www.linuxprobe.com/python-pip-use.html

*pip更换国内源参考文章：*https://dyblogs.cn/dy/2185.html