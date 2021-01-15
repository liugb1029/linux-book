## pidstat 概述
pidstat是sysstat工具的一个命令，用于监控全部或指定进程的cpu、内存、线程、设备IO等系统资源的占用情况。pidstat首次运行时显示自系统启动开始的各项统计信息，之后运行pidstat将显示自上次运行该命令以后的统计信息。用户可以通过指定统计的次数和时间来获得所需的统计信息。

## pidstat 安装
pidstat 是sysstat软件套件的一部分，sysstat包含很多监控linux系统状态的工具，它能够从大多数linux发行版的软件源中获得。

在Debian/Ubuntu系统中可以使用下面的命令来安装:
```bash
apt-get install sysstat
```
CentOS/Fedora/RHEL版本的linux中则使用下面的命令：
```bash
yum install sysstat
```

## pidstat用法以及参数
```bash
[root@master ~]# pidstat --help
Usage: pidstat [ options ] [ <interval> [ <count> ] ]
Options are:
[ -d ] [ -h ] [ -I ] [ -l ] [ -r ] [ -s ] [ -t ] [ -U [ <username> ] ] [ -u ]
[ -V ] [ -w ] [ -C <command> ] [ -p { <pid> [,...] | SELF | ALL } ]
[ -T { TASK | CHILD | ALL } ]
```
常用的参数：
- -u：默认的参数，显示各个进程的cpu使用统计
- -r：显示各个进程的内存使用统计
- -d：显示各个进程的IO使用情况
- -p：指定进程号
- -w：显示每个进程的上下文切换情况
- -t：显示选择任务的线程的统计信息外的额外信息
- -T { TASK | CHILD | ALL }
这个选项指定了pidstat监控的。TASK表示报告独立的task，CHILD关键字表示报告进程下所有线程统计信息。ALL表示报告独立的task和task下面的所有线程。
注意：task和子线程的全局的统计信息和pidstat选项无关。这些统计信息不会对应到当前的统计间隔，这些统计信息只有在子线程kill或者完成的时候才会被收集。
- -V：版本号
- -h：在一行上显示了所有活动，这样其他程序可以容易解析。
- -I：在SMP环境，表示任务的CPU使用率/内核数量
- -l：显示命令名和所有参数

## 示例
### 示例一：查看所有进程的 CPU 使用情况（ -u -p ALL）
pidstat 和 pidstat -u -p ALL 是等效的。
pidstat 默认显示了所有进程的cpu使用率。
```bash
[root@master ~]# pidstat -u -p ALL
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:06:00 PM   UID       PID    %usr %system  %guest    %CPU   CPU  Command
05:06:00 PM     0         1    0.01    0.02    0.00    0.03     0  systemd
05:06:00 PM     0         2    0.00    0.00    0.00    0.00     1  kthreadd
05:06:00 PM     0         3    0.00    0.00    0.00    0.00     0  ksoftirqd/0
05:06:00 PM     0         5    0.00    0.00    0.00    0.00     0  kworker/0:0H
05:06:00 PM     0         7    0.00    0.00    0.00    0.00     0  migration/0
05:06:00 PM     0         8    0.00    0.00    0.00    0.00     1  rcu_bh
05:06:00 PM     0         9    0.00    0.01    0.00    0.01     1  rcu_sched
05:06:00 PM     0        10    0.00    0.00    0.00    0.00     0  lru-add-drain
05:06:00 PM     0        11    0.00    0.00    0.00    0.00     0  watchdog/0
05:06:00 PM     0        12    0.00    0.00    0.00    0.00     1  watchdog/1
05:06:00 PM     0        13    0.00    0.00    0.00    0.00     1  migration/1
05:06:00 PM     0        14    0.00    0.00    0.00    0.00     1  ksoftirqd/1
```
### 示例二: cpu使用情况统计(-u)
```bash
pidstat -u
```
使用-u选项，pidstat将显示各活动进程的cpu使用统计，执行”pidstat -u”与单独执行”pidstat”的效果一样。


### 示例三： 内存使用情况统计(-r)
使用-r选项，pidstat将显示各活动进程的内存使用统计：
```bash
[root@master ~]# pidstat -r
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:09:00 PM   UID       PID  minflt/s  majflt/s     VSZ    RSS   %MEM  Command
05:09:00 PM     0         1      0.38      0.00  125324   3804   0.20  systemd
05:09:00 PM     0       469      0.16      0.00   39076   2892   0.15  systemd-journal
05:09:00 PM     0       491      0.02      0.00  190332   1340   0.07  lvmetad
05:09:00 PM     0       505      0.14      0.00   44552   2112   0.11  systemd-udevd
05:09:00 PM     0       619      0.01      0.00   55512    904   0.05  auditd
05:09:00 PM     0       641      1.42      0.00   21524   1216   0.06  irqbalance
05:09:00 PM    81       643      0.04      0.00   58196   2488   0.13  dbus-daemon
05:09:00 PM   999       647      0.09      0.00  538436  14144   0.75  polkitd
05:09:00 PM     0       650      0.14      0.00  478668   9332   0.50  NetworkManager
05:09:00 PM     0       651      0.05      0.00   26376   1768   0.09  systemd-logind
05:09:00 PM     0       657      0.06      0.00  126284   1616   0.09  crond
05:09:00 PM     0       660      0.02      0.00  110088    860   0.05  agetty
05:09:00 PM     0       862      0.40      0.00  573848  19008   1.01  tuned
05:09:00 PM     0       863      0.05      0.00  112892   4304   0.23  sshd
05:09:00 PM     0       865      0.06      0.00  222716   4564   0.24  rsyslogd
05:09:00 PM     0       867      0.67      0.01  508244  59916   3.19  dockerd
05:09:00 PM     0      1022      0.29      0.01  279220  30504   1.62  docker-containe
```
- PID：进程标识符
- Minflt/s:任务每秒发生的次要错误，不需要从磁盘中加载页
- Majflt/s:任务每秒发生的主要错误，需要从磁盘中加载页
- VSZ：虚拟地址大小，虚拟内存的使用KB
- RSS：常驻集合大小，非交换区五里内存使用KB
- Command：task命令名

### 示例四： 显示各个进程的IO使用情况（-d）
```bash
[root@master ~]# pidstat -d
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:10:34 PM   UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s  Command
05:10:34 PM     0         1      3.95      0.18      0.03  systemd
05:10:34 PM     0       469      0.04      0.00      0.00  systemd-journal
05:10:34 PM     0       491      0.00      0.00      0.00  lvmetad
05:10:34 PM     0       505      0.44      0.00      0.00  systemd-udevd
05:10:34 PM     0       619      0.00      0.02      0.00  auditd
05:10:34 PM     0       641      0.00      0.00      0.00  irqbalance
05:10:34 PM    81       643      0.04      0.00      0.00  dbus-daemon
05:10:34 PM   999       647      0.25      0.00      0.00  polkitd
05:10:34 PM     0       650      0.44      0.00      0.00  NetworkManager
05:10:34 PM     0       651      0.02      0.00      0.00  systemd-logind
05:10:34 PM     0       657      0.03      0.01      0.00  crond
05:10:34 PM     0       660      0.00      0.00      0.00  agetty
05:10:34 PM     0       862      0.33      0.00      0.00  tuned
05:10:34 PM     0       863      1.52      0.00      0.00  sshd
05:10:34 PM     0       865      0.04      0.04      0.02  rsyslogd
05:10:34 PM     0       867      2.76      0.04      0.00  dockerd
05:10:34 PM     0      1022      1.87      0.01      0.00  docker-containe
05:10:34 PM     0      1156      0.04      0.00      0.00  master
05:10:34 PM    89      1167      0.01      0.00      0.00  qmgr
05:10:34 PM     0     12014      4.39      0.06      0.00  bash
05:10:34 PM     0     16743      0.00      0.00      0.00  nginx
```
报告IO统计显示以下信息：
- PID：进程id
- kB_rd/s：每秒从磁盘读取的KB
- kB_wr/s：每秒写入磁盘KB
- kB_ccwr/s：任务取消的写入磁盘的KB。当任务截断脏的pagecache的时候会发生。
- COMMAND:task的命令名

### 示例五：显示每个进程的上下文切换情况（-w）
```bash
[root@master ~]# pidstat -w -p 867
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:12:13 PM   UID       PID   cswch/s nvcswch/s  Command
05:12:13 PM     0       867      4.85      0.01  dockerd
```
- PID:进程id
- Cswch/s:每秒主动任务上下文切换数量
- Nvcswch/s:每秒被动任务上下文切换数量
- Command:命令名


### 示例六：显示选择任务的线程的统计信息外的额外信息 (-t)
```bash
[root@master ~]# pidstat -t -p 867
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:13:19 PM   UID      TGID       TID    %usr %system  %guest    %CPU   CPU  Command
05:13:19 PM     0       867         -    0.11    0.11    0.00    0.22     0  dockerd
05:13:19 PM     0         -       867    0.02    0.01    0.00    0.03     0  |__dockerd
05:13:19 PM     0         -       968    0.01    0.04    0.00    0.05     1  |__dockerd
05:13:19 PM     0         -       969    0.00    0.00    0.00    0.00     0  |__dockerd
05:13:19 PM     0         -       970    0.00    0.00    0.00    0.00     0  |__dockerd
05:13:19 PM     0         -       971    0.00    0.00    0.00    0.00     1  |__dockerd
05:13:19 PM     0         -       993    0.02    0.01    0.00    0.03     1  |__dockerd
05:13:19 PM     0         -      1012    0.00    0.00    0.00    0.00     0  |__dockerd
05:13:19 PM     0         -      1067    0.01    0.01    0.00    0.03     0  |__dockerd
```

### 示例七： pidstat -T
TASK表示报告独立的task。
CHILD关键字表示报告进程下所有线程统计信息。
ALL表示报告独立的task和task下面的所有线程。

```bash
[root@master ~]# pidstat -T ALL -p 867
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:14:44 PM   UID       PID    %usr %system  %guest    %CPU   CPU  Command
05:14:44 PM     0       867    0.11    0.11    0.00    0.22     1  dockerd

05:14:44 PM   UID       PID    usr-ms system-ms  guest-ms  Command
05:14:44 PM     0       867     29910     29100         0  dockerd
```
- PID:进程id
- Usr-ms:任务和子线程在用户级别使用的毫秒数。
- System-ms:任务和子线程在系统级别使用的毫秒数。
- Guest-ms:任务和子线程在虚拟机(running a virtual processor)使用的毫秒数。
- Command:命令名

### 示例八： 显示出命令以及所有参数(-l)
```bash
[root@master ~]# pidstat -l
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:18:05 PM   UID       PID    %usr %system  %guest    %CPU   CPU  Command
05:18:05 PM     0         1    0.01    0.02    0.00    0.03     0  /usr/lib/systemd/systemd --switched-root --system --deserialize 22
05:18:05 PM     0         2    0.00    0.00    0.00    0.00     0  kthreadd
05:18:05 PM     0         3    0.00    0.00    0.00    0.00     0  ksoftirqd/0
05:18:05 PM     0         7    0.00    0.00    0.00    0.00     0  migration/0
05:18:05 PM     0         9    0.00    0.01    0.00    0.01     1  rcu_sched
05:18:05 PM     0        11    0.00    0.00    0.00    0.00     0  watchdog/0
05:18:05 PM     0        12    0.00    0.00    0.00    0.00     1  watchdog/1
05:18:05 PM     0        13    0.00    0.00    0.00    0.00     1  migration/1
05:18:05 PM     0        14    0.00    0.00    0.00    0.00     1  ksoftirqd/1
05:18:05 PM     0        20    0.00    0.00    0.00    0.00     1  khungtaskd
05:18:05 PM     0        34    0.00    0.00    0.00    0.00     1  khugepaged
05:18:05 PM     0       293    0.00    0.00    0.00    0.00     1  kworker/u64:5
05:18:05 PM     0       331    0.00    0.00    0.00    0.00     1  kworker/1:1H
05:18:05 PM     0       400    0.00    0.01    0.00    0.01     0  xfsaild/dm-0
05:18:05 PM     0       469    0.00    0.00    0.00    0.00     1  /usr/lib/systemd/systemd-journald
05:18:05 PM     0       505    0.00    0.00    0.00    0.00     1  /usr/lib/systemd/systemd-udevd
05:18:05 PM     0       619    0.00    0.00    0.00    0.00     1  /sbin/auditd
05:18:05 PM     0       641    0.00    0.01    0.00    0.01     1  /usr/sbin/irqbalance --foreground
05:18:05 PM    81       643    0.00    0.00    0.00    0.00     1  /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation

```




