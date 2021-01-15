## mpstat

### mpstat简介
　　mpstat是一个实时监控工具，主要报告与CPU相关统计信息，信息存放在/proc/stat文件中；

　　在多核心cpu系统中，不仅可以查看cpu平均信息，还可以查看指定cpu信息

### mpstat执行格式
　　mpstat [ -A ] [ -u ] [ -V ] [ -I { keyword [,...] | ALL } ] [ -P { cpu [,...] | ON | ALL } ] [ interval [ count ] ]

### mpstat选项
　　-A： 等同于 -u -I ALL -P ALL

　　-u： 报告CPU利用率。将显示以下值

　　　　CPU: 处理器编号。关键字all表示统计信息计算为所有处理器之间的平均值。

　　　　％usr: 显示在用户级（应用程序）执行时发生的CPU利用率百分比。

　　　　％nice: 显示以优先级较高的用户级别执行时发生的CPU利用率百分比。

　　　　％sys: 显示在系统级（内核）执行时发生的CPU利用率百分比。请注意，这不包括维护硬件和软件的时间中断。

　　　　％iowait: 显示系统具有未完成磁盘I / O请求的CPU或CPU空闲的时间百分比。

　　　　％irq: 显示CPU或CPU用于服务硬件中断的时间百分比。

　　　　%soft: 显示CPU或CPU用于服务软件中断的时间百分比。

　　　　%steal: 显示虚拟CPU或CPU在管理程序为另一个虚拟处理器提供服务时非自愿等待的时间百分比。

　　　　%guest: 显示CPU或CPU运行虚拟处理器所花费的时间百分比。

　　　　%gnice: 显示CPU或CPU运行niced客户机所花费的时间百分比。

　　　　%idle: 显示CPU或CPU空闲且系统没有未完成的磁盘I / O请求的时间百分比。

　　-V ： 打印版本号，然后退出

　　-I {SUM | CPU | ALL | SCPU} ：报告中断统计信息。 使用SUM关键字，mpstat命令报告每个处理器的中断总数。使用CPU关键字，显示CPU或CPU每秒接收的每个中断的数量。ALL关键字等效于指定上面的所有关键字，因此显示所有中断统计信息。SCPU显示软中断的统计信息

　　interval：指定每个报告之间的时间（不指定count则持续生成报告）

　　count：指定生成报告数量

### 示例
#### 1. 不加参数：显示所有CPU整体使用状态
```bash
[root@master ~]# mpstat
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:32:17 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
05:32:17 PM  all    0.26    0.00    0.43    0.00    0.00    0.00    0.00    0.00    0.00   99.30
```
#### 2. -P ALL|0: 显示指定CPU使用状态
```bash
[root@master ~]# mpstat -P ALL
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:33:01 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
05:33:01 PM  all    0.26    0.00    0.43    0.00    0.00    0.00    0.00    0.00    0.00   99.30
05:33:01 PM    0    0.27    0.00    0.43    0.00    0.00    0.00    0.00    0.00    0.00   99.30
05:33:01 PM    1    0.26    0.00    0.43    0.01    0.00    0.00    0.00    0.00    0.00   99.31
[root@master ~]#
[root@master ~]# mpstat -P 0
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:33:03 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
05:33:03 PM    0    0.27    0.00    0.43    0.00    0.00    0.00    0.00    0.00    0.00   99.30
[root@master ~]#
[root@master ~]# mpstat -P 1
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:33:04 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
05:33:04 PM    1    0.26    0.00    0.43    0.01    0.00    0.00    0.00    0.00    0.00   99.31
```

#### 3. -I SUM -P ALL|0: 查看所有CPU或者指定CPU中断统计
```bash
[root@master ~]# mpstat -I SUM -P ALL
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:35:52 PM  CPU    intr/s
05:35:52 PM  all    338.22
05:35:52 PM    0    159.58
05:35:52 PM    1    178.64
```

#### 4. -I CPU：查看CPU每秒接收每个中断的次数；如果中断太多会导致显示器显示错乱。可以查找指定中断的次数
```bash
[root@master ~]# mpstat -I CPU
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:35:07 PM  CPU        0/s        1/s        8/s        9/s       12/s       14/s       15/s       18/s       19/s       22/s       24/s       25/s       26/s       27/s       28/s       29/s       30/s       31/s      NMI/s      LOC/s      SPU/s      PMI/s      IWI/s      RTR/s      RES/s      CAL/s      TLB/s      TRM/s      THR/s      DFR/s      MCE/s      MCP/s      ERR/s      MIS/s      PIN/s      NPI/s      PIW/s
05:35:07 PM    0       0.00       0.00       0.00       0.92       0.01       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.13       0.00       0.27       0.01       0.00     124.88       0.00       0.00       0.39       0.00      32.86       0.02       0.07       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00
05:35:07 PM    1       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       1.91       0.00       0.65       0.00       0.00       0.00       0.00     139.56       0.00       0.00       2.04       0.00      34.34       0.03       0.10       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00
[root@master ~]# mpstat -I ALL
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:35:25 PM  CPU    intr/s
05:35:25 PM  all    338.21

05:35:25 PM  CPU        0/s        1/s        8/s        9/s       12/s       14/s       15/s       18/s       19/s       22/s       24/s       25/s       26/s       27/s       28/s       29/s       30/s       31/s      NMI/s      LOC/s      SPU/s      PMI/s      IWI/s      RTR/s      RES/s      CAL/s      TLB/s      TRM/s      THR/s      DFR/s      MCE/s      MCP/s      ERR/s      MIS/s      PIN/s      NPI/s      PIW/s
05:35:25 PM    0       0.00       0.00       0.00       0.92       0.01       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.13       0.00       0.27       0.01       0.00     124.88       0.00       0.00       0.39       0.00      32.86       0.02       0.07       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00
05:35:25 PM    1       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       1.91       0.00       0.65       0.00       0.00       0.00       0.00     139.56       0.00       0.00       2.04       0.00      34.34       0.03       0.10       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00       0.00

05:35:25 PM  CPU       HI/s    TIMER/s   NET_TX/s   NET_RX/s    BLOCK/s BLOCK_IOPOLL/s  TASKLET/s    SCHED/s  HRTIMER/s      RCU/s
05:35:25 PM    0       0.00      13.85       0.00       0.00       0.13       0.00       0.00      11.49       0.00       3.93
05:35:25 PM    1       0.00      26.43       0.00       1.91       0.65       0.00       0.00      17.90       0.00       7.19
```

#### 5.查找指定中断的次数： 比如中断252的每秒次数
(1)、查找所在列数
```bash
mpstat -I CPU|awk '/252/{for(i=1;i<NF;i++)if($i~/252/)print$i,i}'
252  23
```

(2)、根据列数查找252全部CPU中断次数
```bash
[root@master ~]# mpstat -I CPU|awk '{print $1,$2,$3,$23}'
Linux 3.10.0-862.el7.x86_64 (master)

05:43:23 PM CPU LOC/s
05:43:23 PM 0 124.82
05:43:23 PM 1 139.54
```

#### 6. 查看软中断统计信息
```bash
[root@master ~]# mpstat -I SCPU
Linux 3.10.0-862.el7.x86_64 (master) 	12/03/2020 	_x86_64_	(2 CPU)

05:45:25 PM  CPU       HI/s    TIMER/s   NET_TX/s   NET_RX/s    BLOCK/s BLOCK_IOPOLL/s  TASKLET/s    SCHED/s  HRTIMER/s      RCU/s
05:45:25 PM    0       0.00      13.82       0.00       0.00       0.13       0.00       0.00      11.46       0.00       3.94
05:45:25 PM    1       0.00      26.42       0.00       1.92       0.65       0.00       0.00      17.88       0.00       7.19
[root@master ~]#
```

#### 7. 