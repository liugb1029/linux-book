## CenOS7 中文显示

### 一、查看是否存在中文语言包
```
[root@centos7-1810 ~]# locale -a | grep zh_CN
zh_CN
zh_CN.gb18030
zh_CN.gb2312
zh_CN.gbk
zh_CN.utf8
```
此处显示存在，这里我们使用zh_CN.utf8语言包。

### 二、设置系统语言为中文
使用命令
```bash
localectl set-locale LANG=zh_CN.UTF-8
```
或
修改配置文件/etc/locale.conf为
```bash
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN.UTF-8"
```

```bash
source /etc/locale.conf
```

如不提示错误表示已开启中文，两种方法作用一样都是修改配置文件，重新登录终端生效。

### 三、查看更改后的系统语言变量
```bash
[root@master ~]# locale
LANG=zh_CN.UTF-8
LC_CTYPE="zh_CN.utf-8"
LC_NUMERIC="zh_CN.utf-8"
LC_TIME="zh_CN.utf-8"
LC_COLLATE="zh_CN.utf-8"
LC_MONETARY="zh_CN.utf-8"
LC_MESSAGES="zh_CN.utf-8"
LC_PAPER="zh_CN.utf-8"
LC_NAME="zh_CN.utf-8"
LC_ADDRESS="zh_CN.utf-8"
LC_TELEPHONE="zh_CN.utf-8"
LC_MEASUREMENT="zh_CN.utf-8"
LC_IDENTIFICATION="zh_CN.utf-8"
LC_ALL=zh_CN.utf-8
```
四、测试中文语言设置是否成功
在终端随便输入一个不存在的命令：
```bash
[root@localhost ~]# aaaaaa
-bash: aaaaaa: 未找到命令
[root@localhost ~]#
```




