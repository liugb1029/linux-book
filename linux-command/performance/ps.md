## ps简介

ps命令是Process Status的缩写。ps命令用来列出系统中当前运行的那些进程。ps命令列出的是当前那些进程的快照，就是执行ps命令的那个时刻的那些进程，如果想要动态的显示进程信息，就可以使用top命令。

要对进程进行监测和控制，首先必须要了解当前进程的情况，也就是需要查看当前进程，而 ps 命令就是最基本同时也是非常强大的进程查看命令。使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多的资源等等。总之大部分信息都是可以通过执行该命令得到的。

ps 为我们提供了进程的一次性的查看，它所提供的查看结果并不动态连续的；如果想对进程时间监控，应该用 top 工具。kill 命令用于杀死进程。

## ps命令参数

|参数|	描述|
|-|-|
|-A	|列出所有的行程|
|-e	|等于“-A”|
|-a	|显示现行终端机下的所有进程，包括其他用户的进程；|
|-u	|以用户为主的进程状态 ；|
|x	|通常与 a 这个参数一起使用，可列出较完整信息。|
|-w	|显示加宽可以显示较多的资讯|
|-au |显示较详细的资讯|
|-aux |显示所有包含其他使用者的行程|
|-f |做一个更为完整的输出。|

- -a：显示所有终端机下执行的程序，除了阶段作业领导者之外。
- a：显示现行终端机下的所有程序，包括其他用户的程序。
- -A：显示所有程序。
- -c：显示CLS和PRI栏位。
- c：列出程序时，显示每个程序真正的指令名称，而不包含路径，选项或常驻服务的标示。
- -C<指令名称>：指定执行指令的名称，并列出该指令的程序的状况。
- -d：显示所有程序，但不包括阶段作业领导者的程序。
- -e：此选项的效果和指定"A"选项相同。
- e：列出程序时，显示每个程序所使用的环境变量。
- -f：显示UID,PPIP,C与STIME栏位。
- f：用ASCII字符显示树状结构，表达程序间的相互关系。
- -g<群组名称>：此选项的效果和指定"-G"选项相同，当亦能使用阶段作业领导者的名称来指定。
- g：显示现行终端机下的所有程序，包括群组领导者的程序。
- -G<群组识别码>：列出属于该群组的程序的状况，也可使用群组名称来指定。
- h：不显示标题列。
- -H：显示树状结构，表示程序间的相互关系。
- -j或j：采用工作控制的格式显示程序状况。
- -l或l：采用详细的格式来显示程序状况。
- L：列出栏位的相关信息。
- -m或m：显示所有的执行绪。
- n：以数字来表示USER和WCHAN栏位。
- -N：显示所有的程序，除了执行ps指令终端机下的程序之外。
- -p<程序识别码>：指定程序识别码，并列出该程序的状况。
- p<程序识别码>：此选项的效果和指定"-p"选项相同，只在列表格式方面稍有差异。
- r：只列出现行终端机正在执行中的程序。
- -s<阶段作业>：指定阶段作业的程序识别码，并列出隶属该阶段作业的程序的状况。
- s：采用程序信号的格式显示程序状况。
- S：列出程序时，包括已中断的子程序资料。
- -t<终端机编号>：指定终端机编号，并列出属于该终端机的程序的状况。
- t<终端机编号>：此选项的效果和指定"-t"选项相同，只在列表格式方面稍有差异。
- -T：显示现行终端机下的所有程序。
- -u<用户识别码>：此选项的效果和指定"-U"选项相同。
- u：以用户为主的格式来显示程序状况。
- -U<用户识别码>：列出属于该用户的程序的状况，也可使用用户名称来指定。
- U<用户名称>：列出属于该用户的程序的状况。
- v：采用虚拟内存的格式显示程序状况。
- -V或V：显示版本信息。
- -w或w：采用宽阔的格式来显示程序状况。　
- x：显示所有程序，不以终端机来区分。
- X：采用旧式的Linux i386登陆格式显示程序状况。
- -y：配合选项"-l"使用时，不显示F(flag)栏位，并以RSS栏位取代ADDR栏位　。
- -<程序识别码>：此选项的效果和指定"p"选项相同。
- --cols<每列字符数>：设置每列的最大字符数。
- --columns<每列字符数>：此选项的效果和指定"--cols"选项相同。
- --cumulative：此选项的效果和指定"S"选项相同。
- --deselect：此选项的效果和指定"-N"选项相同。
- --forest：此选项的效果和指定"f"选项相同。
- --headers：重复显示标题列。
- --help：在线帮助。
- --info：显示排错信息。
- --sort: 排序
- --lines<显示列数>：设置显示画面的列数。ßß
- --no-headers：此选项的效果和指定"h"选项相同，只在列表格式方面稍有差异。
- --group<群组名称>：此选项的效果和指定"-G"选项相同。
- --Group<群组识别码>：此选项的效果和指定"-G"选项相同。
- --pid<程序识别码>：此选项的效果和指定"-p"选项相同。
- --rows<显示列数>：此选项的效果和指定"--lines"选项相同。
- --sid<阶段作业>：此选项的效果和指定"-s"选项相同。
- --tty<终端机编号>：此选项的效果和指定"-t"选项相同。
- --user<用户名称>：此选项的效果和指定"-U"选项相同。
- --User<用户识别码>：此选项的效果和指定"-U"选项相同。
- --version：此选项的效果和指定"-V"选项相同。
- --widty<每列字符数>：此选项的效果和指定"-cols"选项相同。

## 示例
### 1. 显示所有进程信息(-A)
```bash
[root@cargo compass]# ps -A
  PID TTY          TIME CMD
    1 ?        01:46:45 systemd
    2 ?        00:00:01 kthreadd
    4 ?        00:00:00 kworker/0:0H
    6 ?        00:02:31 ksoftirqd/0
    7 ?        00:00:56 migration/0
    8 ?        00:00:00 rcu_bh
    9 ?        01:12:54 rcu_sched
   10 ?        00:00:00 lru-add-drain
   11 ?        00:00:18 watchdog/0
   12 ?        00:00:11 watchdog/1
   13 ?        00:01:17 migration/1
   14 ?        00:01:57 ksoftirqd/1
   16 ?        00:00:00 kworker/1:0H
   17 ?        00:00:15 watchdog/2
   18 ?        00:00:53 migration/2
   19 ?        00:02:25 ksoftirqd/2
   21 ?        00:00:00 kworker/2:0H
   22 ?        00:00:11 watchdog/3
   23 ?        00:01:19 migration/3
   24 ?        00:02:00 ksoftirqd/3
   26 ?        00:00:00 kworker/3:0H
   .....
```
### 2. 显示指定用户进程信息(-u)
```bash
[root@cargo compass]# ps -u root
  PID TTY          TIME CMD
    1 ?        01:46:45 systemd
    2 ?        00:00:01 kthreadd
    4 ?        00:00:00 kworker/0:0H
    6 ?        00:02:31 ksoftirqd/0
    7 ?        00:00:56 migration/0
    8 ?        00:00:00 rcu_bh
    9 ?        01:12:54 rcu_sched
   10 ?        00:00:00 lru-add-drain
   11 ?        00:00:18 watchdog/0
   12 ?        00:00:11 watchdog/1
   13 ?        00:01:17 migration/1
   14 ?        00:01:57 ksoftirqd/1
   16 ?        00:00:00 kworker/1:0H
   17 ?        00:00:15 watchdog/2
   18 ?        00:00:53 migration/2
   19 ?        00:02:25 ksoftirqd/2
   21 ?        00:00:00 kworker/2:0H
   22 ?        00:00:11 watchdog/3
   23 ?        00:01:19 migration/3
   24 ?        00:02:00 ksoftirqd/3
   26 ?        00:00:00 kworker/3:0H
```
### 3. 显示所有进程信息，连带命令行
```bash
[root@cargo compass]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Jul13 ?        01:46:45 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2     0  0 Jul13 ?        00:00:01 [kthreadd]
root         4     2  0 Jul13 ?        00:00:00 [kworker/0:0H]
root         6     2  0 Jul13 ?        00:02:31 [ksoftirqd/0]
root         7     2  0 Jul13 ?        00:00:56 [migration/0]
root         8     2  0 Jul13 ?        00:00:00 [rcu_bh]
root         9     2  0 Jul13 ?        01:12:54 [rcu_sched]
root        10     2  0 Jul13 ?        00:00:00 [lru-add-drain]
root        11     2  0 Jul13 ?        00:00:18 [watchdog/0]
root        12     2  0 Jul13 ?        00:00:11 [watchdog/1]
root        13     2  0 Jul13 ?        00:01:17 [migration/1]
root        14     2  0 Jul13 ?        00:01:57 [ksoftirqd/1]
root        16     2  0 Jul13 ?        00:00:00 [kworker/1:0H]
root        17     2  0 Jul13 ?        00:00:15 [watchdog/2]
root        18     2  0 Jul13 ?        00:00:53 [migration/2]
root        19     2  0 Jul13 ?        00:02:25 [ksoftirqd/2]
root        21     2  0 Jul13 ?        00:00:00 [kworker/2:0H]
root        22     2  0 Jul13 ?        00:00:11 [watchdog/3]
root        23     2  0 Jul13 ?        00:01:19 [migration/3]
root        24     2  0 Jul13 ?        00:02:00 [ksoftirqd/3]
root        26     2  0 Jul13 ?        00:00:00 [kworker/3:0H]
root        28     2  0 Jul13 ?        00:00:00 [kdevtmpfs]
root        29     2  0 Jul13 ?        00:00:00 [netns]
root        30     2  0 Jul13 ?        00:00:08 [khungtaskd]
root        31     2  0 Jul13 ?        00:00:00 [writeback]
root        32     2  0 Jul13 ?        00:00:00 [kintegrityd]
root        33     2  0 Jul13 ?        00:00:00 [bioset]

```
### 4、将目前属于您自己这次登入的PID与相关信息列出来
```bash
[root@cargo compass]# ps -l
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 R     0 12162 25891  0  80   0 - 38304 -      pts/3    00:00:00 ps
4 S     0 25891 25885  0  80   0 - 29676 do_wai pts/3    00:00:00 bash
[root@cargo compass]#
```
说明：
各相关信息的意义：
|标志	|意义|
|-|-|
|F	|代表这个程序的旗标 (flag)， 4 代表使用者为 super user|
|S	|代表这个程序的状态 (STAT)，关于各 STAT 的意义将在内文介绍|
|UID	|程序被该 UID 所拥有|
|PID	|就是这个程序的 ID ！|
|PPID	|则是其上级父程序的ID|
|C	|CPU 使用的资源百分比|
|PRI	|指进程的执行优先权(Priority的简写)，其值越小越早被执行；|
|NI	|这个进程的nice值，其表示进程可被执行的优先级的修正数值。|
|ADDR	|这个是内核函数，指出该程序在内存的那个部分。如果是个 running的程序，一般就是 "-"|
|SZ	|使用掉的内存大小|
|WCHAN	|目前这个程序是否正在运作当中，若为 - 表示正在运作|
|TTY	|登入者的终端机位置|
|TIME	|使用掉的 CPU 时间。|
|CMD	|所下达的指令为何|
在预设的情况下， ps 仅会列出与目前所在的 bash shell 有关的 PID 而已，所以， 当我使用 ps -l 的时候，只有三个 PID

### 5、列出目前所有的正在内存中的程序(ps aux)
```bash
[root@cargo compass]# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 191504  3424 ?        Ss   Jul13 106:46 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2  0.0  0.0      0     0 ?        S    Jul13   0:01 [kthreadd]
root         4  0.0  0.0      0     0 ?        S<   Jul13   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S    Jul13   2:31 [ksoftirqd/0]
root         7  0.0  0.0      0     0 ?        S    Jul13   0:56 [migration/0]
root         8  0.0  0.0      0     0 ?        S    Jul13   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    Jul13  72:54 [rcu_sched]
root        10  0.0  0.0      0     0 ?        S<   Jul13   0:00 [lru-add-drain]
root        11  0.0  0.0      0     0 ?        S    Jul13   0:18 [watchdog/0]
root        12  0.0  0.0      0     0 ?        S    Jul13   0:11 [watchdog/1]
root        13  0.0  0.0      0     0 ?        S    Jul13   1:17 [migration/1]
root        14  0.0  0.0      0     0 ?        S    Jul13   1:57 [ksoftirqd/1]
root        16  0.0  0.0      0     0 ?        S<   Jul13   0:00 [kworker/1:0H]
root        17  0.0  0.0      0     0 ?        S    Jul13   0:15 [watchdog/2]
root        18  0.0  0.0      0     0 ?        S    Jul13   0:53 [migration/2]
root        19  0.0  0.0      0     0 ?        S    Jul13   2:25 [ksoftirqd/2]
root        21  0.0  0.0      0     0 ?        S<   Jul13   0:00 [kworker/2:0H]
……省略部分结果
```
说明：
|标志	|意义|
|-|-|
|USER	|该 process 属于那个使用者账号的|
|PID	|该 process 的号码|
|%CPU	|该 process 使用掉的 CPU 资源百分比|
|%MEM	|该 process 所占用的物理内存百分比|
|VSZ	|该 process 使用掉的虚拟内存量 (Kbytes)|
|RSS	|该 process 占用的固定的内存量 (Kbytes)|
|TTY	|该 process 是在那个终端机上面运作，若与终端机无关，则显示 ?，另外， tty1-tty6 是本机上面的登入者程序，若为 pts/0 等等的，则表示为由网络连接进主机的程序。|
|STAT	|该程序目前的状态|
|START	|该 process 被触发启动的时间|
|TIME	|该 process 实际使用 CPU 运作的时间|
|COMMAND	|该程序的实际指令|

### 6、以类似进程树的结构显示
```bash
[root@cargo compass]# ps -axjf
 PPID   PID  PGID   SID TTY      TPGID STAT   UID   TIME COMMAND
    0     2     0     0 ?           -1 S        0   0:01 [kthreadd]
    2     4     0     0 ?           -1 S<       0   0:00  \_ [kworker/0:0H]
    2     6     0     0 ?           -1 S        0   2:31  \_ [ksoftirqd/0]
    2     7     0     0 ?           -1 S        0   0:56  \_ [migration/0]
    2     8     0     0 ?           -1 S        0   0:00  \_ [rcu_bh]
    2     9     0     0 ?           -1 S        0  72:54  \_ [rcu_sched]
    2    10     0     0 ?           -1 S<       0   0:00  \_ [lru-add-drain]
    2   742     0     0 ?           -1 S<       0   0:00  \_ [bioset]
    2   743     0     0 ?           -1 S<       0   0:00  \_ [xfsalloc]
    2   744     0     0 ?           -1 S<       0   0:00  \_ [xfs_mru_cache]
    2   745     0     0 ?           -1 S<       0   0:00  \_ [xfs-buf/dm-0]
    2   746     0     0 ?           -1 S<       0   0:00  \_ [xfs-data/dm-0]
    2   747     0     0 ?           -1 S<       0   0:00  \_ [xfs-conv/dm-0]
    2   748     0     0 ?           -1 S<       0   0:00  \_ [xfs-cil/dm-0]
    2   749     0     0 ?           -1 S<       0   0:00  \_ [xfs-reclaim/dm-]
    2   750     0     0 ?           -1 S<       0   0:00  \_ [xfs-log/dm-0]
    2   751     0     0 ?           -1 S<       0   0:00  \_ [xfs-eofblocks/d]
    2   752     0     0 ?           -1 S        0  21:56  \_ [xfsaild/dm-0]
    2  7724     0     0 ?           -1 S        0   0:03  \_ [kworker/1:1]
    2 13268     0     0 ?           -1 S        0   0:04  \_ [kworker/3:1]
    2 28479     0     0 ?           -1 S        0   0:02  \_ [kworker/0:1]
    2 21106     0     0 ?           -1 S        0   0:01  \_ [kworker/u8:0]
    2  9186     0     0 ?           -1 S        0   0:00  \_ [kworker/1:2]
    2  5358     0     0 ?           -1 S        0   0:00  \_ [kworker/u8:2]
    2 30683     0     0 ?           -1 S        0   0:00  \_ [kworker/0:0]
    2  9266     0     0 ?           -1 S        0   0:00  \_ [kworker/3:2]
    2  6694     0     0 ?           -1 S        0   0:00  \_ [kworker/2:1]
    2 15817     0     0 ?           -1 S        0   0:00  \_ [kworker/2:2]
    2 20459     0     0 ?           -1 S        0   0:00  \_ [kworker/2:0]
    0     1     1     1 ?           -1 Ss       0 106:46 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
    1   492   492   492 ?           -1 Ss       0 206:09 /usr/lib/systemd/systemd-journald
    1   510   510   510 ?           -1 Ss       0   0:00 /usr/sbin/lvmetad -f
    1   526   526   526 ?           -1 Ss       0   0:00 /usr/lib/systemd/systemd-udevd
    1   770   770   770 ?           -1 S<sl     0   0:08 /sbin/auditd
    1   819   819   819 ?           -1 Ss      81  24:05 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activa
    1   840   840   840 ?           -1 Ssl    999   0:23 /usr/lib/polkit-1/polkitd --no-debug
    1   841   841   841 ?           -1 Ss       0   8:19 /usr/lib/systemd/systemd-logind
    1   845   845   845 ?           -1 Ss       0   0:00 /usr/sbin/atd -f
    1   851   851   851 ?           -1 Ss       0   0:13 /usr/sbin/crond -n
    1   867   867   867 tty1       867 Ss+      0   0:00 /sbin/agetty --noclear tty1 linux
    1   869   869   869 ttyS0      869 Ss+      0   0:00 /sbin/agetty --keep-baud 115200,38400,9600 ttyS0 vt220
    1  1021  1021  1021 ?           -1 Ss       0   0:00 /sbin/dhclient -1 -q -lf /var/lib/dhclient/dhclient--eth0.lease -pf /var/run/dhclient-
    1  1083  1083  1083 ?           -1 Ssl      0  13:25 /usr/bin/python2 -Es /usr/sbin/tuned -l -P
    1  1086  1086  1086 ?           -1 Ssl      0  77:51 /usr/sbin/rsyslogd -n
    1  1099  1099  1099 ?           -1 Ssl      0 443:55 /usr/bin/containerd
 1099 10971 10971  1099 ?           -1 Sl       0  59:33  \_ containerd-shim -namespace moby -workdir /var/lib/containerd/io.containerd.runtime
10971 10989 10989 10989 ?           -1 Ss       0   0:01  |   \_ /bin/bash /assets/wrapper
10989 11042 10989 10989 ?           -1 S        0   1:04  |       \_ runsvdir -P /opt/gitlab/service log: .....................................
11042 11049 11049 11049 ?           -1 Ss       0   0:00  |       |   \_ runsv sshd
....省略部分结果
```

### 7、指定输出哪些列(-o)
```bash
[root@cargo compass]# ps -eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,wchan:14,comm
  PID   TID CLS RTPRIO  NI PRI PSR %CPU STAT WCHAN          COMMAND
    1     1 TS       -   0  19   0  0.0 Ss   ep_poll        systemd
    2     2 TS       -   0  19   2  0.0 S    kthreadd       kthreadd
    4     4 TS       - -20  39   0  0.0 S<   worker_thread  kworker/0:0H
    6     6 TS       -   0  19   0  0.0 S    smpboot_thread ksoftirqd/0
    7     7 FF      99   - 139   0  0.0 S    smpboot_thread migration/0
    8     8 TS       -   0  19   0  0.0 S    rcu_gp_kthread rcu_bh
    9     9 TS       -   0  19   3  0.0 S    rcu_gp_kthread rcu_sched
   10    10 TS       - -20  39   0  0.0 S<   rescuer_thread lru-add-drain
   11    11 FF      99   - 139   0  0.0 S    smpboot_thread watchdog/0
   12    12 FF      99   - 139   1  0.0 S    smpboot_thread watchdog/1
   13    13 FF      99   - 139   1  0.0 S    smpboot_thread migration/1
   14    14 TS       -   0  19   1  0.0 S    smpboot_thread ksoftirqd/1
   16    16 TS       - -20  39   1  0.0 S<   worker_thread  kworker/1:0H
   17    17 FF      99   - 139   2  0.0 S    smpboot_thread watchdog/2
   18    18 FF      99   - 139   2  0.0 S    smpboot_thread migration/2
   19    19 TS       -   0  19   2  0.0 S    smpboot_thread ksoftirqd/2
   21    21 TS       - -20  39   2  0.0 S<   worker_thread  kworker/2:0H
   22    22 FF      99   - 139   3  0.0 S    smpboot_thread watchdog/3
```
### 8、输出cpu top5的进程
```bash
# cpu 位于输出列第3个
[root@cargo compass]# ps -aux|sort -k3 -rn|head -n5
chrony   11329  5.3  9.7 1266060 757100 ?      Ssl  Jul31 9696:26 puma 4.3.3.gitlab.2 (tcp://localhost:9168) [gitlab-exporter]
root     14120  2.1  0.3 148800 24696 ?        S<sl Nov19 464:28 /usr/local/aegis/aegis_client/aegis_10_89/AliYunDun
chrony    2171  2.1  8.7 1686728 673052 ?      Sl   Aug22 3287:40 sidekiq 5.2.9 queues:authorized_project_update:authorized_project_update_project_create,authorized_project_update:authorized_project_update_project_group_link_create,authorized_project_update:authorized_project_update_user_refresh_over_user_range,authorized_project_update:authorized_project_update_user_refresh_with_low_urgency,auto_devops:auto_devops_disable,auto_merge:auto_merge_process,chaos:chaos_cpu_spin,chaos:chaos_db_spin,chaos:chaos_kill,chaos:chaos_leak_mem,chaos:chaos_sleep,container_repository:cleanup_container_repository,container_repository:delete_container_repository,cronjob:admin_email,cronjob:authorized_project_update_periodic_recalculate,cronjob:ci_archive_traces_cron,cronjob:container_expiration_policy,cronjob:environments_auto_stop_cron,cronjob:expire_build_artifacts,cronjob:gitlab_usage_ping,cronjob:import_export_project_cleanup,cronjob:import_stuck_project_import_jobs,cronjob:issue_due_scheduler,cronjob:jira_import_stuck_jira_import_jobs,cronjob:metrics_dashboard_schedule_annotations_prun
996      14504  0.8  0.1  63148 11788 ?        Ss   Jul31 1508:13 postgres: gitlab gitlabhq_production [local] idle
997      11321  0.7  0.0  69864  6712 ?        Ssl  Jul31 1441:08 /opt/gitlab/embedded/bin/redis-server 127.0.0.1:0
```

### 9、输出内存 top5的进程
```bash
[root@cargo curator]# ps -aux|sort -k4 -rn|head -n5
chrony    6343  0.2  9.9 1482656 766128 ?      Sl   02:52   1:19 puma: cluster worker 0: 322 [gitlab-puma-worker]
chrony   11329  5.3  9.7 1266060 757100 ?      Ssl  Jul31 9696:37 puma 4.3.3.gitlab.2 (tcp://localhost:9168) [gitlab-exporter]
chrony    8075  0.2  9.3 1482656 722732 ?      Sl   02:54   1:23 puma: cluster worker 2: 322 [gitlab-puma-worker]
chrony    5510  0.2  9.1 1499040 703828 ?      Sl   02:51   1:30 puma: cluster worker 3: 322 [gitlab-puma-worker]
chrony    7178  0.2  9.0 1483680 697280 ?      Sl   02:53   1:28 puma: cluster worker 1: 322 [gitlab-puma-worker]
```

### 10、排序(--sort)
```bash
# 按照CPU排序
[root@cargo ~]# ps aux --sort -pcpu
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
chrony   11329  5.3  9.7 1266060 757100 ?      Ssl  Jul31 9697:15 puma 4.3.3.gitlab.2 (tcp://localhost:9168) [gitlab-exporter]
chrony    2171  2.1  8.7 1686728 675168 ?      Sl   Aug22 3287:59 sidekiq 5.2.9 queues:authorized_project_update:authorized_project_update_proj
root     14120  2.1  0.3 148800 24696 ?        S<sl Nov19 464:53 /usr/local/aegis/aegis_client/aegis_10_89/AliYunDun
996      14504  0.8  0.1  63148 11788 ?        Ss   Jul31 1508:21 postgres: gitlab gitlabhq_production [local] idle
997      11321  0.7  0.0  69864  6572 ?        Ssl  Jul31 1441:15 /opt/gitlab/embedded/bin/redis-server 127.0.0.1:0
992      11345  0.7  2.7 2238976 211072 ?      Ssl  Jul31 1314:25 /opt/gitlab/embedded/bin/prometheus --web.listen-address=localhost:9090 --sto
root      1099  0.2  0.4 1180612 34424 ?       Ssl  Jul13 443:58 /usr/bin/containerd
chrony    5510  0.2  9.1 1499040 705132 ?      Sl   02:51   1:33 puma: cluster worker 3: 322 [gitlab-puma-worker]
chrony    6343  0.2  9.7 1482656 752564 ?      Sl   02:52   1:20 puma: cluster worker 0: 322 [gitlab-puma-worker]
# 按照mem排序
[root@cargo ~]# ps aux --sort -pmem
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
chrony   11329  5.3  9.7 1266060 757100 ?      Ssl  Jul31 9697:18 puma 4.3.3.gitlab.2 (tcp://localhost:9168) [gitlab-exporter]
chrony    6343  0.2  9.7 1482656 756320 ?      Sl   02:52   1:20 puma: cluster worker 0: 322 [gitlab-puma-worker]
chrony    8075  0.2  9.2 1482656 716664 ?      Sl   02:54   1:26 puma: cluster worker 2: 322 [gitlab-puma-worker]
chrony    5510  0.2  9.1 1499040 707512 ?      Sl   02:51   1:33 puma: cluster worker 3: 322 [gitlab-puma-worker]
chrony    7178  0.2  9.0 1483680 699788 ?      Sl   02:53   1:29 puma: cluster worker 1: 322 [gitlab-puma-worker]
chrony    2171  2.1  8.7 1686728 674352 ?      Sl   Aug22 3288:00 sidekiq 5.2.9 queues:authorized_project_update:authorized_project_update_proj
chrony   11344  0.0  8.3 1167248 644816 ?      Ssl  Jul31  29:13 puma 4.3.3.gitlab.2 (unix:///var/opt/gitlab/gitlab-rails/sockets/gitlab.socket
polkitd   7372  0.0  3.1 1664132 242940 ?      Ssl  Nov16   9:24 mysqld
```

### 11、根据线程来过滤进程(-L)
```bash
[root@cargo ~]# ps -L 28572
  PID   LWP TTY      STAT   TIME COMMAND
28572 28572 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
28572 28573 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
28572 28574 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
28572 28575 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
28572 28576 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
28572 28577 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
28572 28578 ?        Sl     0:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -container-port 80
[root@cargo ~]#
```
### 12、根据进程名和pid过滤(-C)
```bash
[root@cargo ~]# ps -f -C docker-proxy
UID        PID  PPID  C STIME TTY          TIME CMD
root      7348  7625  0 Nov16 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 13306 -container-ip 172.18.0.4 -co
root     10936  7625  0 Jul31 ?        00:00:02 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 2443 -container-ip 172.18.0.3 -con
root     10950  7625  0 Jul31 ?        00:00:02 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 80 -container-ip 172.18.0.3 -conta
root     10964  7625  0 Jul31 ?        00:00:03 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 222 -container-ip 172.18.0.3 -cont
root     18699  7625  0 Nov13 ?        00:02:24 /usr/bin/docker-proxy -proto tcp -host-ip 127.0.0.1 -host-port 1514 -container-ip 172.19.0.2 -c
root     19723  7625  0 Nov13 ?        00:00:11 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 443 -container-ip 172.19.0.11 -con
root     28572  7625  0 Nov13 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 3142 -container-ip 172.18.0.2 -con
[root@cargo ~]#
```
### 13、使用PS实时监控进程状态

ps 命令会显示你系统当前的进程状态，但是这个结果是静态的。

当有一种情况，我们需要像上面第四点中提到的通过CPU和内存的使用率来筛选进程，并且我们希望结果能够每秒刷新一次。为此，我们可以将ps命令和watch命令结合起来。

```bash
[root@cargo ~]# watch -n 1 'ps -aux --sort -pmem,-pcpu'
Every 1.0s: ps -aux --sort -pmem,-pcpu                                                                                 Fri Dec  4 12:05:27 2020

USER	   PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
chrony   11329  5.3  9.7 1266060 757100 ?      Ssl  Jul31 9697:50 puma 4.3.3.gitlab.2 (tcp://localhost:9168) [gitlab-exporter]
chrony    5510  0.2  9.3 1499040 719256 ?      Sl   02:51   1:33 puma: cluster worker 3: 322 [gitlab-puma-worker]
chrony    6343  0.2  9.2 1482656 714624 ?      Sl   02:52   1:23 puma: cluster worker 0: 322 [gitlab-puma-worker]
chrony    8075  0.2  9.1 1482656 708772 ?      Sl   02:54   1:27 puma: cluster worker 2: 322 [gitlab-puma-worker]
chrony    7178  0.2  9.1 1483680 707704 ?      Sl   02:53   1:31 puma: cluster worker 1: 322 [gitlab-puma-worker]
chrony    2171  2.1  8.7 1686728 677044 ?      Sl   Aug22 3288:12 sidekiq 5.2.9 queues:authorized_project_update:authorized_project_update_proj
chrony   11344  0.0  8.3 1167248 646136 ?      Ssl  Jul31  29:13 puma 4.3.3.gitlab.2 (unix:///var/opt/gitlab/gitlab-rails/sockets/gitlab.socket
polkitd   7372  0.0  3.1 1664132 242940 ?      Ssl  Nov16   9:25 mysqld
992	 11345  0.7  2.7 2238976 213056 ?      Ssl  Jul31 1314:30 /opt/gitlab/embedded/bin/prometheus --web.listen-address=localhost:9090 --sto
root	   492  0.0  1.9 260356 151276 ?       Ss   Jul13 206:12 /usr/lib/systemd/systemd-journald
polkitd  20473  0.0  1.3 181444 104284 ?       Ss   Nov13   0:03 postgres: checkpointer process
polkitd  20557  0.0  1.1 184780 89208 ?        Ss   Nov13   3:17 postgres: postgres postgres 172.19.0.8(56222) idle
polkitd  16873  0.0  1.0 184908 81072 ?        Ss   Nov14   3:01 postgres: postgres postgres 172.19.0.8(58102) idle
```

### 14、格式化输出root用户(真实的或有效的UID)创建的进程
```bash
ps -U root -u root u
```
- -U 参数按真实用户ID(RUID)筛选进程，它会从用户列表中选择真实用户名或 ID。真实用户即实际创建该进程的用户。
- -u 参数用来筛选有效用户ID（EUID）。
最后的u参数用来决定以针对用户的格式输出，由User, PID, %CPU, %MEM, VSZ, RSS, TTY, STAT, START, TIME 和 COMMAND这几列组成
```bash
[root@clever-cargo ~]# ps -U root -u root u
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0 191504  3424 ?        Ss   Jul13 106:47 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2  0.0  0.0      0     0 ?        S    Jul13   0:01 [kthreadd]
root         4  0.0  0.0      0     0 ?        S<   Jul13   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S    Jul13   2:31 [ksoftirqd/0]
root         7  0.0  0.0      0     0 ?        S    Jul13   0:56 [migration/0]
root         8  0.0  0.0      0     0 ?        S    Jul13   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    Jul13  72:55 [rcu_sched]
root        10  0.0  0.0      0     0 ?        S<   Jul13   0:00 [lru-add-drain]
root        11  0.0  0.0      0     0 ?        S    Jul13   0:18 [watchdog/0]
root        12  0.0  0.0      0     0 ?        S    Jul13   0:11 [watchdog/1]
root        13  0.0  0.0      0     0 ?        S    Jul13   1:17 [migration/1]
root        14  0.0  0.0      0     0 ?        S    Jul13   1:57 [ksoftirqd/1]
root        16  0.0  0.0      0     0 ?        S<   Jul13   0:00 [kworker/1:0H]
root        17  0.0  0.0      0     0 ?        S    Jul13   0:15 [watchdog/2]
root        18  0.0  0.0      0     0 ?        S    Jul13   0:53 [migration/2]
root        19  0.0  0.0      0     0 ?        S    Jul13   2:25 [ksoftirqd/2]
root        21  0.0  0.0      0     0 ?        S<   Jul13   0:00 [kworker/2:0H]
root        22  0.0  0.0      0     0 ?        S    Jul13   0:11 [watchdog/3]
root        23  0.0  0.0      0     0 ?        S    Jul13   1:19 [migration/3]
```
### 15、显示安全信息
如果想要查看现在有谁登入了你的服务器。可以使用ps命令加上相关参数:
参数 -e 显示所有进程信息，-o 参数控制输出。Pid,User 和 Args参数显示PID，运行应用的用户和该应用。
```bash
[root@clever-cargo ~]# ps -eo pid,user,args
  PID USER     COMMAND
    1 root     /usr/lib/systemd/systemd --switched-root --system --deserialize 22
    2 root     [kthreadd]
    4 root     [kworker/0:0H]
    6 root     [ksoftirqd/0]
    7 root     [migration/0]
    8 root     [rcu_bh]
    9 root     [rcu_sched]
   10 root     [lru-add-drain]
   11 root     [watchdog/0]
    1021 root     /sbin/dhclient -1 -q -lf /var/lib/dhclient/dhclient--eth0.lease -pf /var/run/dhclient-eth0.pid -H clever-cargo eth0
 1083 root     /usr/bin/python2 -Es /usr/sbin/tuned -l -P
 1086 root     /usr/sbin/rsyslogd -n
 1099 root     /usr/bin/containerd
 1325 root     /usr/sbin/sshd -D
 2156 chrony   ruby /opt/gitlab/embedded/service/gitlab-rails/bin/sidekiq-cluster -e production -r /opt/gitlab/embedded/service/gitlab-rails -m
 2171 chrony   sidekiq 5.2.9 queues:authorized_project_update:authorized_project_update_project_create,authorized_project_update:authorized_pro
 3663 chrony   ruby /opt/gitlab/embedded/service/gitaly-ruby/bin/gitaly-ruby 359 /var/opt/gitlab/gitaly/internal_sockets/ruby.0
 4120 root     ./ossutil cp -r oss://infra-release/platform/clever/clever-v1.7.0/ /compass/
 5358 root     [kworker/u8:2]

```


