## linux命令学习

### 1.diff

- 对比文件或者目录

- -q：仅当文件不同时报告
- -r：递归比较找到的所有子目录

#### 案例

```shell
 diff -qr py36 py36.ori/ | grep -E ^Only | grep -V py36.ori  >> py36diff.txt
```

### 2.sort

- 排序

### 3.uniq

- 去重

#### 4.while死循环访问网站进行测试

```shell
export a=0 && while :; do let a=$a+a ; echo $a ; curl -I xxxx ; done
```



