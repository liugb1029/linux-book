## vmstat

### 安装
Linux 中没有独立的`vmstat`包。它与`sysstat`绑定在一起，并在大多数发行版的默认仓库上都有。如果还没有安装，只要基于你的发行版输入下面的命令。
```bash
# [在 CentOS/RHEL 中安装 vmstat]
$ sudo yum install sysstat
# [在 Fedora 中安装 vmstat]
$ sudo dnf install sysstat
# [在 Debian/Ubuntu 中安装 vmstat]
$ sudo apt-get install sysstat
# [在 Arch Linux 中安装 vmstat]
$ sudo pacman -S sysstat
# [在 Mageia 中安装 vmstat]
$ sudo urpmi sysstat
# [在 openSUSE 中安装 vmstat]
$ sudo zypper install sysstat
```

### 用法
```bash
vmstat [-a] [-n] [-S unit] [delay [ count]]
vmstat [-s] [-n] [-S unit]
vmstat [-m] [-n] [delay [ count]]
vmstat [-d] [-n] [delay [ count]]
vmstat [-p disk partition] [-n] [delay [ count]]
vmstat [-f]
vmstat [-V]
```
- -a：显示活跃和非活跃内存
- -f：显示从系统启动至今的fork数量 。
- -m：显示slabinfo
- -n：只在开始时显示一次各字段名称.
- -s：显示内存相关统计信息及多种系统活动数量。
- delay：刷新时间间隔。如果不指定，只显示一条结果。
- count：刷新次数。如果不指定刷新次数，但指定了刷新时间间隔，这时刷新次数为无穷。
- -d：显示磁盘相关统计信息。
- -p：显示指定磁盘分区统计信息
- -S：使用指定单位显示。参数有 k 、K 、m 、M ，分别代表1000、1024、1000000、1048576字节（byte）。默认单位为K（1024 bytes）
- -V：显示vmstat版本信息。
- -w：输出格式，以空格填充。

### 字段说明

当你看到上面的输出，你可能已经大致了解了它是什么以及它的目的。不要担心，我们将深入解释每个参数，以便你可以了解 vmstat 的用途和目的。

**procs**：procs 中有**r**和**b**列，它报告进程统计信息。在上面的输出中，在运行队列（r）中有两个进程在等待 CPU 并有零个休眠进程（b）。通常，它不应该超过处理器（或核心）的数量，如果你发现异常，最好使用 top 命令进一步地排除故障。
- r：等待运行的进程数。
- b：休眠状态下的进程数。

**memory**： memory 下有报告内存统计的 swapd、free、buff 和 cache 列。你可以用 free -m 命令看到同样的信息。在上面的内存统计中，统计数据以千字节表示，这有点难以理解，最好添加 M 参数来看到以兆字节为单位的统计数据。
- swapd：使用的虚拟内存量。
- free：空闲内存量。
- buff：用作缓冲区的内存量。
- cache：用作高速缓存的内存量。
- inact：非活动内存的数量。
- active：活动内存量。
  
**swap**：swap 有 si 和 so 列，用于报告交换内存统计信息。你可以用 free -m 命令看到相同的信息。
- si：从磁盘交换的内存量（换入，从 swap 移到实际内存的内存）。
- so：交换到磁盘的内存量（换出，从实际内存移动到 swap 的内存）。

**I/O**：I/O 有 bi 和 bo 列，它以“块读取”和“块写入”的单位来报告每秒磁盘读取和写入的块的统计信息。如果你发现有巨大的 I/O 读写，最好使用 iotop 和 iostat 命令来查看。
- bi：从块设备接收的块数。
- bo：发送到块设备的块数。

**system**：system 有 in 和 cs 列，它报告每秒的系统操作。
- in：每秒的系统中断数，包括时钟中断。
- cs：系统为了处理所以任务而上下文切换的数量。
CPU：CPU 有 us、sy、id 和 wa 列，报告（所用的） CPU 资源占总 CPU 时间的百分比。如果你发现异常，最好使用 top 和 free 命令。
- us：处理器在非内核程序消耗的时间。
- sy：处理器在内核相关任务上消耗的时间。
- id：处理器的空闲时间。
- wa：处理器在等待IO操作完成以继续处理任务上的时间。



|类别| 项目 | 含义 |说明|
|-|-|-|-|
|Procs（进程）|r|等待执行的任务数|展示了正在执行和等待cpu资源的任务个数。当这个值超过了cpu个数，就会出现cpu瓶颈。|
| |B|等待IO的进程数量|如果该值一直都很大，说明IO比较繁忙|
|Memory(内存)|swpd|正在使用虚拟的内存大小，单位k||
||free|空闲内存大小||
||buff|已用的buff大小，对块设备的读写进行缓冲||
||cache|已用的cache大小，文件系统的cache||
||inact|非活跃内存大小，即被标明可回收的内存，区别于free和active|具体含义见：概念补充（当使用-a选项时显示）|
||active|活跃的内存大小|具体含义见：概念补充（当使用-a选项时显示）|
|Swap|si|每秒从交换区写入内存的大小（单位：kb/s）||
||so|每秒从内存写到交换区的大小||
|IO|bi|每秒读取的块数（读磁盘）|现在的Linux版本块的大小为1024bytes|
||bo|每秒写入的块数（写磁盘）||
|system|in|每秒中断数，包括时钟中断|这两个值越大，会看到由内核消耗的cpu时间会越多
||cs|每秒上下文切换数||
|CPU（以百分比表示)|Us|用户进程执行消耗cpu时间(user time)|us的值比较高时，说明用户进程消耗的cpu时间多，但是如果长期超过50%的使用，那么我们就该考虑优化程序算法或其他措施了|
||Sy|系统进程消耗cpu时间(system time)|sys的值过高时，说明系统内核消耗的cpu资源多，这个不是良性的表现，我们应该检查原因。|
||Id|空闲时间(包括IO等待时间)||
||wa|等待IO时间|Wa过高时，说明io等待比较严重，这可能是由于磁盘大量随机访问造成的，也有可能是磁盘的带宽出现瓶颈。|
||st|被虚拟机所盗用的CPU百分比|||


### 案例
#### 1、以MB方式输出
默认情况下，vmstat 以千字节为单位显示内存统计，这是非常难以理解的，最好添加 -S m 参数以获取以兆字节为单位的统计。
```bash
# vmstat -S m
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 1  0    103    371    406   2116    0    0    40    15    0    0 11  1 87  0
```

#### 2、以延迟方式运行 vmstat 获取更好的统计信息
默认情况下，vmstat 的单次统计信息不足以进一步进行故障排除，因此，添加更新延迟（延迟是更新之间的延迟，以秒为单位）以定期捕获活动。如果你想以 2 秒延迟运行 vmstat ，只需使用下面的命令（如果你想要更长的延迟，你可以根据你的愿望改变）。

以下命令将每 2 秒运行一次，直到退出。
```bash
# vmstat 2
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 1  0 105500 325776 416016 2166912   0    0    40    15    0    0 11  1 87  0
 0  0 105500 325644 416016 2166920   0    0     0    13 1083 1174 11  1 87  0
 0  0 105500 308648 416024 2166928   0    0     1    16 1559 1453 16  2 82  0
 0  0 105500 285948 416032 2166932   0    0     0    12  934 1003  9  1 90  0
 0  0 105500 326620 416040 2166940   0    0     1    27  922 1068  9  1 90  0
 0  0 105500 366704 416048 2166944   0    0     0    17  835  955  9  1 90  0
 0  0 105500 366456 416056 2166948   0    0     1    22  859  918  9  1 90  0
 0  0 105500 366456 416056 2166948   0    0     0    15 1539 1504 17  2 81  0
 0  0 105500 365224 416060 2166996   0    0     1    19  984 1097 11  1 88  0
```
#### 3、带延迟和计数运行 vmstat
或者，你可以带延迟和特定计数运行 vmstat，一旦达到给定的计数，然后自动退出。

以下命令将每 2 秒运行一次，10 次后自动退出。
```bash
# vmstat 2 10
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 1  0  79496 1581916 157380 810412   0    0    23    10    0    1 11  1 88  0
 2  0  79496 1559464 157380 810416   0    0     1     1 1821 1749 21  2 77  0
 0  0  79496 1583768 157384 810416   0    0     1    46  681  799  9  1 90  0
 2  0  79496 1556364 157384 810428   0    0     1     1 1392 1545 15  2 83  0
 0  0  79496 1583272 157384 810428   0    0     1     0 1307 1448 14  2 84  0
 2  0  79496 1582032 157384 810428   0    0     1    41  424  605  4  1 96  0
 1  0  79496 1575848 157384 810428   0    0     1     0 1912 2407 26  2 71  0
 0  0  79496 1582884 157384 810436   0    0     1    69  678  825  9  1 90  0
 2  0  79496 1569368 157392 810432   0    0    11    26  920  969  9  1 90  0
 1  0  79496 1583612 157400 810444   0    0     7    39 2001 2530 20  2 77  0
```
#### 4、显示活动和非活动内存
默认情况下，vmstat 会显示除活动和非活动内存之外的内存统计信息。如果要查看活动和非活动内存统计信息，请在 vmstat 后添加 -a 参数。
```bash
# vmstat -a
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free  inact active   si   so    bi    bo   in   cs us sy id wa
 1  0 105500 2387592 415148 584112   0    0    40    15    0    1 11  1 87  0
```
#### 5、打印磁盘统计
在 vmstat 后面添加 -d 参数会以每个磁盘一行的方式显示统计（包含读、写和 IO）。
```bash
# vmstat -d
disk- ------------reads------------ ------------writes----------- -----IO------
       total merged sectors      ms  total merged sectors      ms    cur    sec
ram0       0      0       0       0      0      0       0       0      0      0
ram1       0      0       0       0      0      0       0       0      0      0
ram2       0      0       0       0      0      0       0       0      0      0
ram3       0      0       0       0      0      0       0       0      0      0
ram4       0      0       0       0      0      0       0       0      0      0
ram5       0      0       0       0      0      0       0       0      0      0
ram6       0      0       0       0      0      0       0       0      0      0
ram7       0      0       0       0      0      0       0       0      0      0
ram8       0      0       0       0      0      0       0       0      0      0
ram9       0      0       0       0      0      0       0       0      0      0
ram10      0      0       0       0      0      0       0       0      0      0
ram11      0      0       0       0      0      0       0       0      0      0
ram12      0      0       0       0      0      0       0       0      0      0
ram13      0      0       0       0      0      0       0       0      0      0
ram14      0      0       0       0      0      0       0       0      0      0
ram15      0      0       0       0      0      0       0       0      0      0
loop0      0      0       0       0      0      0       0       0      0      0
loop1      0      0       0       0      0      0       0       0      0      0
loop2      0      0       0       0      0      0       0       0      0      0
loop3      0      0       0       0      0      0       0       0      0      0
loop4      0      0       0       0      0      0       0       0      0      0
loop5      0      0       0       0      0      0       0       0      0      0
loop6      0      0       0       0      0      0       0       0      0      0
loop7      0      0       0       0      0      0       0       0      0      0
fd0        0      0       0       0      0      0       0       0      0      0
sda   16604050 904497 2594882190 57455732 30037054 28093770 2160032056 118189160      0  40915
sdb   257357577 479985 3124712204 577235320 8502519 1283237 36645890 11250948      0 182336
```
#### 6、总结磁盘统计
在 vmstat 后面添加 -D 会显示全局统计（包括全部的磁盘、分区、全部读、合并的读、读取的扇区、写、合并的写、写入的扇区和 IO）。
```bash
# vmstat -D
           27 disks
            3 partitions
    275754028 total reads
      1388030 merged reads
   5751195976 read sectors
    638710116 milli reading
     38795040 writes
     29520659 merged writes
   2209820333 written sectors
    130210652 milli writing
            0 inprogress IO
       224704 milli spent IO
```
#### 7、打印指定分区统计
vmstat 添加 -p 参数后面跟上设备名会显示指定分区统计（包括读、读取的扇区、写以及请求的写）。
```bash
# vmstat -p /dev/sdb1
sdb1          reads   read sectors  writes    requested writes
                3115      27890     839453  206728016
```
#### 8、vmstat 统计信息带上时间戳
当你想在特定时间区间内找到内存尖峰时，用 vmstat 命令添加 -t 参数，后跟延迟和计数。

注意：此组合不适用于基于 Debian 的系统。
```bash
# vmstat -t 1 5
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------ ---timestamp---
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 6981416 181324 24588604    0    0     0     1    0    0  0  0 100  0  0    2017-01-11 15:42:15 MST
 2  0      0 6981276 181324 24588604    0    0     0     0   91   40  0  0 100  0  0    2017-01-11 15:42:16 MST
 0  0      0 6982016 181324 24588604    0    0     0     0   75  116  0  0 100  0  0    2017-01-11 15:42:17 MST
 0  0      0 6982016 181324 24588604    0    0     0     0   43   39  0  0 100  0  0    2017-01-11 15:42:18 MST
 0  0      0 6982280 181324 24588604    0    0     0     0  113  185  0  0 100  0  0    2017-01-11 15:42:19 MST
```
#### 9、打印更多统计
vmstat 后面跟上 -s 参数会显示不同统计的总结。
```bash
# vmstat -s
     32849392  total memory
     25864128  used memory
     16468180  active memory
      8320888  inactive memory
      6985264  free memory
       181324  buffer memory
     24588612  swap cache
     20970492  total swap
            0  used swap
     20970492  free swap
       891075 non-nice user cpu ticks
         6532 nice user cpu ticks
      1507099 system cpu ticks
  18925265601 idle cpu ticks
       113043 IO-wait cpu ticks
          108 IRQ cpu ticks
         4185 softirq cpu ticks
            0 stolen cpu ticks
      4071862 pages paged in
    216759718 pages paged out
            0 pages swapped in
            0 pages swapped out
    369611221 interrupts
    477861261 CPU context switches
   1478258826 boot time
      2196121 forks
```
####9、打印 slab 统计
vmstat 后面跟上 -m 参数会显示 slab 信息。
```bash
# vmstat -m
Cache                       Num  Total   Size  Pages
nf_conntrack_expect           0      0    240     16
nf_conntrack_ffffffff81b2a920     18     60    312     12
fib6_nodes                   24     59     64     59
ip6_dst_cache                16     30    384     10
ndisc_cache                   7     30    256     15
ip6_mrt_cache                 0      0    128     30
RAWv6                        35     35   1088      7
UDPLITEv6                     0      0   1024      4
UDPv6                         4     12   1024      4
tw_sock_TCPv6                 0      0    320     12
request_sock_TCPv6            0      0    192     20
TCPv6                         4      6   1920      2
fat_inode_cache               5      6    672      6
fat_cache                     0      0     32    112
ioat2                      4096   4140    128     30
ext4_inode_cache          34322  34364   1000      4
ext4_xattr                    0      0     88     44
.
.
.
```

#### 10. 改变输出格式(-w)
```bash
[root@master ~]# vmstat -w
procs -----------------------memory---------------------- ---swap-- -----io---- -system-- --------cpu--------
 r  b         swpd         free         buff        cache   si   so    bi    bo   in   cs  us  sy  id  wa  st
 1  0            0      1266776         2104       459848    0    0     8     1  170  275   0   0  99   0   0
```

