## sar

sar（System Activity Reporter系统活动情况报告）是目前 Linux 上最为全面的系统性能分析工具之一，可以从多方面对系统的活动进行报告，包括：文件的读写情况、系统调用的使用情况、磁盘I/O、CPU效率、内存使用状况、进程活动及IPC有关的活动等。

## 命令格式
sar [options] [-A] [-o file] t [n]

其中：
- t为采样间隔，n为采样次数，默认值是1；
- -o file表示将命令结果以二进制格式存放在文件中，file 是文件名。
options 为命令行选项，sar命令常用选项如下：
- -A：所有报告的总和
- -u：输出CPU使用情况的统计信息
- -v：输出inode、文件和其他内核表的统计信息
- -d：输出每一个块设备的活动信息
- -r：输出内存和交换空间的统计信息
- -b：显示I/O和传送速率的统计信息
- -a：文件读写情况
- -h: 针对网络或者IO读写输出以人可直观识别的方式输出。
- -c：输出进程统计信息，每秒创建的进程数
- -R：输出内存页面的统计信息
- -y：终端设备活动情况
- -w：输出系统交换活动信息

## 示例
### 1. CPU资源监控(-u)
例如，每10秒采样一次，连续采样3次，观察CPU 的使用情况，并将采样结果以二进制形式存入当前目录下的文件test中，需键入如下命令：
```bash
sar -u -o test 10 3
```
```bash
[root@clever-cargo ~]# sar -u -o test 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

02:51:15 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
02:51:25 PM     all      5.34      0.00      1.15      0.03      0.00     93.48
02:51:35 PM     all      4.91      0.00      0.73      0.00      0.00     94.36
02:51:45 PM     all      1.98      0.00      0.50      0.03      0.00     97.49
Average:        all      4.08      0.00      0.79      0.02      0.00     95.11
```
输出项说明：
- CPU：all 表示统计信息为所有 CPU 的平均值。
- %user：显示在用户级别(application)运行使用 CPU 总时间的百分比。
- %nice：显示在用户级别，用于nice操作，所占用 CPU 总时间的百分比。
- %system：在核心级别(kernel)运行所使用 CPU 总时间的百分比。
- %iowait：显示用于等待I/O操作占用 CPU 总时间的百分比。
- %steal：管理程序(hypervisor)为另一个虚拟进程提供服务而等待虚拟 CPU 的百分比。
- %idle：显示 CPU 空闲时间占用 CPU 总时间的百分比。

如果要查看二进制文件test中的内容，需键入如下sar命令：
```bash
sar -u -f test
```

```bash
[root@clever-cargo ~]# sar -u -f test
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

02:51:15 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
02:51:25 PM     all      5.34      0.00      1.15      0.03      0.00     93.48
02:51:35 PM     all      4.91      0.00      0.73      0.00      0.00     94.36
02:51:45 PM     all      1.98      0.00      0.50      0.03      0.00     97.49
Average:        all      4.08      0.00      0.79      0.02      0.00     95.11
[root@clever-cargo ~]#
```

### 2. inode、文件和其他内核表监控(-v)
例如，每10秒采样一次，连续采样3次，观察核心表的状态，需键入如下命令：
```bash
sar -v 10 3
```
```bash
[root@clever-cargo ~]# sar -v 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:01:09 PM dentunusd   file-nr  inode-nr    pty-nr
03:01:19 PM     58713      5888     84573         7
03:01:29 PM     58720      5888     84577         7
03:01:39 PM     58722      5888     84578         7
Average:        58718      5888     84576         7
[root@clever-cargo ~]#
```
输出项说明：
- dentunusd：目录高速缓存中未被使用的条目数量
- file-nr：文件句柄（file handle）的使用数量
- inode-nr：索引节点句柄（inode handle）的使用数量
- pty-nr：使用的pty数量

### 3. 内存和交换空间监控(r)
例如，每10秒采样一次，连续采样3次，监控内存分页：
```bash
sar -r 10 3
```
```bash
[root@clever-cargo ~]# sar -r 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:03:21 PM kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:03:31 PM    620996   7112064     91.97     75988   2207372   7099740     91.81   5303372   1289560       904
03:03:41 PM    617276   7115784     92.02     75996   2207676   7099740     91.81   5306828   1289816       440
03:03:51 PM    613208   7119852     92.07     76008   2207936   7099736     91.81   5310628   1290036       636
Average:       617160   7115900     92.02     75997   2207661   7099739     91.81   5306943   1289804       660
```
输出项说明：
- kbmemfree：这个值和free命令中的free值基本一致,所以它不包括buffer和cache的空间.
- kbmemused：这个值和free命令中的used值基本一致,所以它包括buffer和cache的空间.
- %memused：这个值是kbmemused和内存总量(不包括swap)的一个百分比.
- kbbuffers和kbcached：这两个值就是free命令中的buffer和cache.
- kbcommit：保证当前系统所需要的内存,即为了确保不溢出而需要的内存(RAM+swap).
- %commit：这个值是kbcommit与内存总量(包括swap)的一个百分比.

### 4. 内存分页监控(-B)
例如，每10秒采样一次，连续采样3次，监控内存分页：
```bash
sar -B 10 3
```
```bash
[root@clever-cargo ~]# sar -B 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:08:15 PM  pgpgin/s pgpgout/s   fault/s  majflt/s  pgfree/s pgscank/s pgscand/s pgsteal/s    %vmeff
03:08:25 PM      0.00     60.00    800.50      0.00    649.00      0.00      0.00      0.00      0.00
03:08:35 PM      0.00     68.10  15197.50      0.00   7334.20      0.00      0.00      0.00      0.00
03:08:45 PM      0.00    174.50   6293.40      0.00   3191.30      0.00      0.00      0.00      0.00
Average:         0.00    100.87   7430.47      0.00   3724.83      0.00      0.00      0.00      0.00
```

输出项说明：
- pgpgin/s：表示每秒从磁盘或SWAP置换到内存的字节数(KB)
- pgpgout/s：表示每秒从内存置换到磁盘或SWAP的字节数(KB)
- fault/s：每秒钟系统产生的缺页数,即主缺页与次缺页之和(major + minor)
- majflt/s：每秒钟产生的主缺页数.
- pgfree/s：每秒被放入空闲队列中的页个数
- pgscank/s：每秒被kswapd扫描的页个数
- pgscand/s：每秒直接被扫描的页个数
- pgsteal/s：每秒钟从cache中被清除来满足内存需要的页个数
- %vmeff：每秒清除的页(pgsteal)占总扫描页(pgscank+pgscand)的百分比

### 5. I/O和传送速率监控(-b)
例如，每10秒采样一次，连续采样3次，报告缓冲区的使用情况，需键入如下命令：
```bash
sar -b 10 3
```
```bash
[root@clever-cargo ~]# sar -b 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:10:18 PM       tps      rtps      wtps   bread/s   bwrtn/s
03:10:28 PM      9.40      0.00      9.40      0.00    146.20
03:10:38 PM     29.30      0.00     29.30      0.00    422.30
03:10:48 PM      2.30      0.00      2.30      0.00     87.90
Average:        13.67      0.00     13.67      0.00    218.80
```

输出项说明：
- tps：每秒钟物理设备的 I/O 传输总量
- rtps：每秒钟从物理设备读入的数据总量
- wtps：每秒钟向物理设备写入的数据总量
- bread/s：每秒钟从物理设备读入的数据量，单位为 块/s
- bwrtn/s：每秒钟向物理设备写入的数据量，单位为 块/s

### 6. 进程队列长度和平均负载状态监控(-q)
例如，每10秒采样一次，连续采样3次，监控进程队列长度和平均负载状态：
```bash
sar -q 10 3
```
```bash
[root@clever-cargo ~]# sar -q 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:11:34 PM   runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
03:11:44 PM         0      1054      0.17      0.21      0.18         0
03:11:54 PM         0      1052      0.15      0.20      0.18         0
03:12:04 PM         1      1052      0.27      0.22      0.19         0
Average:            0      1053      0.20      0.21      0.18         0
[root@clever-cargo ~]#
```

输出项说明：
- runq-sz：运行队列的长度（等待运行的进程数）
- plist-sz：进程列表中进程（processes）和线程（threads）的数量
- ldavg-1：最后1分钟的系统平均负载（System load average）
- ldavg-5：过去5分钟的系统平均负载
- ldavg-15：过去15分钟的系统平均负载

### 7. 系统交换活动信息监控(-W)
```bash
sar -W 10 3
```
```bash
[root@clever-cargo ~]# sar -W 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:13:46 PM  pswpin/s pswpout/s
03:13:56 PM      0.00      0.00
03:14:06 PM      0.00      0.00
03:14:16 PM      0.00      0.00
Average:         0.00      0.00
```
输出项说明：
- pswpin/s    每秒从交换分区到系统的交换页面（swap page）数量
- pswpott/s   每秒从系统交换到swap的交换页面（swap page）的数量

### 8. 查看磁盘使用情况(-d)
DEV 磁盘设备的名称，如果不加-p，会显示dev253-0类似的设备名称，因此加上-p显示的名称更直接
```bash
sar -d 10 3
```
```bash
[root@clever-cargo ~]# sar -d -p 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:16:41 PM       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
03:16:51 PM       vda     20.90      0.00    402.40     19.25      0.08      4.02      0.07      0.15
03:16:51 PM       vdb      0.80      0.00     14.30     17.88      0.00      0.25      0.12      0.01
03:16:51 PM compass-cargo      0.60      0.00     14.30     23.83      0.00      0.33      0.33      0.02

03:16:51 PM       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
03:17:01 PM       vda      1.60      0.00     47.20     29.50      0.00      0.50      0.31      0.05
03:17:01 PM       vdb      3.30      0.00     48.90     14.82      0.00      0.39      0.21      0.07
03:17:01 PM compass-cargo      2.30      0.00     48.90     21.26      0.00      0.61      0.61      0.14

03:17:01 PM       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
03:17:11 PM       vda     22.30      0.00    354.40     15.89      0.05      2.28      0.05      0.12
03:17:11 PM       vdb      5.20      0.00    114.10     21.94      0.01      1.42      0.08      0.04
03:17:11 PM compass-cargo      6.20      0.00    114.10     18.40      0.01      1.61      0.10      0.06

Average:          DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
Average:          vda     14.93      0.00    268.00     17.95      0.04      3.03      0.07      0.11
Average:          vdb      3.10      0.00     59.10     19.06      0.00      0.96      0.13      0.04
Average:    compass-cargo      3.03      0.00     59.10     19.48      0.00      1.27      0.24      0.07
```
输出说明：
- DEV  磁盘设备的名称，如果不加-p，会显示dev253-0类似的设备名称，因此加上-p显示的名称更直接
- tps  每秒I/O的传输总数
- rd_sec/s  每秒读取的扇区的总数
- wr_sec/s  每秒写入的扇区的总数
- avgrq-sz  平均每次次磁盘I/O操作的数据大小（扇区）
- avgqu-sz  磁盘请求队列的平均长度
- await  从请求磁盘操作到系统完成处理，每次请求的平均消耗时间，包括请求队列等待时间，单位是毫秒（1秒等于1000毫秒），等于寻道时间+队列时间+服务时间
- svctm  I/O的服务处理时间，即不包括请求队列中的时间
- %util  I/O请求占用的CPU百分比，值越高，说明I/O越慢

### 9. 统计网络信息(-n)
```
    -n { <关键词> [,...] | ALL }
        关键词可以是：
        DEV    网卡
        EDEV    网卡 (错误)
        NFS    NFS 客户端
        NFSD    NFS 服务器
        SOCK    Sockets (套接字)    (v4)
        IP    IP 流    (v4)
        EIP    IP 流    (v4) (错误)
        ICMP    ICMP 流    (v4)
        EICMP    ICMP 流    (v4) (错误)
        TCP    TCP 流    (v4)
        ETCP    TCP 流    (v4) (错误)
        UDP    UDP 流    (v4)
        SOCK6    Sockets (套接字)    (v6)
        IP6    IP 流    (v6)
        EIP6    IP 流    (v6) (错误)
        ICMP6    ICMP 流    (v6)
        EICMP6    ICMP 流    (v6) (错误)
        UDP6    UDP 流    (v6)
```

#### 9.1 网络接口信息(-n DEV)
```bash
sar -n DEV 10 3
```
```bash
[root@clever-cargo ~]# sar -n DEV 10 3
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:21:00 PM     IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s
03:21:10 PM veth54874d0      4.70      3.20      0.41      0.44      0.00      0.00      0.00
03:21:10 PM veth97366cc      1.70      0.70      0.18      0.07      0.00      0.00      0.00
03:21:10 PM veth64373eb      0.70      0.50      0.05      0.05      0.00      0.00      0.00
03:21:10 PM veth54224ad      0.40      0.80      0.11      0.35      0.00      0.00      0.00
03:21:10 PM veth4ad1c5c      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:21:10 PM veth2f5cf06      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:21:10 PM vethf119248      0.80      0.80      0.05      0.33      0.00      0.00      0.00
03:21:10 PM veth7c88a3f      8.40     11.60      0.62      6.80      0.00      0.00      0.00
03:21:10 PM veth456a815     10.90      8.30      6.78      0.63      0.00      0.00      0.00
03:21:10 PM      eth0      2.00      2.60      0.42      0.41      0.00      0.00      0.00
03:21:10 PM br-9cc9e6083cb3      2.20      0.80      0.12      0.33      0.00      0.00      0.00
03:21:10 PM        lo      2.40      2.40      0.40      0.40      0.00      0.00      0.00
03:21:10 PM veth7117093      0.40      0.60      0.04      0.05      0.00      0.00      0.00
03:21:10 PM vethc967ed0      0.10      0.20      0.02      0.02      0.00      0.00      0.00
03:21:10 PM veth31438fd      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:21:10 PM veth1bf228d      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:21:10 PM veth70401b1      0.40      0.60      0.12      0.05      0.00      0.00      0.00
03:21:10 PM veth2a59ee8      0.20      0.40      0.03      0.03      0.00      0.00      0.00
03:21:10 PM   docker0      0.40      0.80      0.11      0.35      0.00      0.00      0.00
```
输出项目说明：
- IFACE  本地网卡接口的名称
- rxpck/s  每秒钟接收的数据包
- txpck/s  每秒钟发送的数据包
- rxkB/S  每秒钟接收的数据包大小，单位为KB
- txkB/S  每秒钟发送的数据包大小，单位为KB
- rxcmp/s  每秒钟接收的压缩数据包
- txcmp/s  每秒钟发送的压缩包
- rxmcst/s  每秒钟接收的多播数据包

#### 9.2 网络设备通信失败信息(-n EDEV)
```bash
sar -n EDEV 10 3
```
```bash
[root@clever-cargo ~]# sar -n EDEV 1 1
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:34:58 PM     IFACE   rxerr/s   txerr/s    coll/s  rxdrop/s  txdrop/s  txcarr/s  rxfram/s  rxfifo/s  txfifo/s
03:34:59 PM veth54874d0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth97366cc      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth64373eb      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth54224ad      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth4ad1c5c      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth2f5cf06      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM vethf119248      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth7c88a3f      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth456a815      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM      eth0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM br-9cc9e6083cb3      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth7117093      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM vethc967ed0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth31438fd      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth1bf228d      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth70401b1      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM veth2a59ee8      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:34:59 PM   docker0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
```
输出说明：
- IFACE 网卡名称
- rxerr/s  每秒钟接收到的损坏的数据包
- txerr/s  每秒钟发送的数据包错误数
- coll/s  当发送数据包时候，每秒钟发生的冲撞（collisions）数，这个是在半双工模式下才有
- rxdrop/s 当由于缓冲区满的时候，网卡设备接收端每秒钟丢掉的网络包的数目
- txdrop/s 当由于缓冲区满的时候，网络设备发送端每秒钟丢掉的网络包的数目
- txcarr/s 当发送数据包的时候，每秒钟载波错误发生的次数
- rxfram/s 在接收数据包的时候，每秒钟发生的帧对其错误的次数
- rxfifo/s 在接收数据包的时候，每秒钟缓冲区溢出的错误发生的次数
- txfifo/s 在发生数据包 的时候，每秒钟缓冲区溢出的错误发生的次数


#### 9.3 统计socket连接信息  sar -n SOCK
```bash
sar -n SOCK 10 3
```
```bash
[root@clever-cargo ~]# sar -n SOCK 1 1
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:38:04 PM    totsck    tcpsck    udpsck    rawsck   ip-frag    tcp-tw
03:38:05 PM       964        48         3         0         0        62
Average:          964        48         3         0         0        62
```
输出项说明：
- totsck  当前被使用的socket总数
- tcpsck  当前正在被使用的TCP的socket总数
- udpsck   当前正在被使用的UDP的socket总数
- rawsck  当前正在被使用于RAW的skcket总数
- if-frag   当前的IP分片的数目
- tcp-tw  TCP套接字中处于TIME-WAIT状态的连接数量

#### 9.4 TCP连接的统计  sar -n TCP 
```bash
sar -n TCP 10 3
```
```bash
[root@clever-cargo ~]# sar -n TCP 1 1
Linux 3.10.0-1062.12.1.el7.x86_64 (clever-cargo) 	12/04/2020 	_x86_64_	(4 CPU)

03:40:10 PM  active/s passive/s    iseg/s    oseg/s
03:40:11 PM      0.00      0.00      2.00      0.00
Average:         0.00      0.00      2.00      0.00
[root@clever-cargo ~]#
```
输出项说明：
- active/s  新的主动连接
- passive/s  新的被动连接
- iseg/s  接受的段
- oseg/s  输出的段

## 总结
```bash
sar 1 1     //  CPU和IOWAIT统计状态  默认监控
sar -b 1 1        // IO传送速率
sar -B 1 1        // 页交换速率
sar -c 1 1        // 进程创建的速率
sar -d 1 1        // 块设备的活跃信息
sar -n DEV 1 1    // 网路设备的状态信息
sar -n SOCK 1 1   // SOCK的使用情况
sar -n ALL 1 1    // 所有的网络状态信息
sar -P ALL 1 1    // 每颗CPU的使用状态信息和IOWAIT统计状态 
sar -q 1 1        // 队列的长度（等待运行的进程数）和负载的状态
sar -r 1 1      // 内存和swap空间使用情况
sar -R 1 1       // 内存的统计信息（内存页的分配和释放、系统每秒作为BUFFER使用内存页、每秒被cache到的内存页）
sar -u 1 1       // CPU的使用情况和IOWAIT信息（同默认监控）
sar -v 1 1       // inode, file and other kernel tablesd的状态信息
sar -w 1 1       // 每秒上下文交换的数目
sar -W 1 1       // SWAP交换的统计信息(监控状态同iostat 的si so)
sar -x 2906 1 1  // 显示指定进程(2906)的统计信息，信息包括：进程造成的错误、用户级和系统级用户CPU的占用情况、运行在哪颗CPU上
sar -y 1 1       // TTY设备的活动状态
sar -o / -f 将输出到文件(-o)和读取记录信息(-f)
```




