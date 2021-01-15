# 目录

* [全书组织](Introduction.md)

## Part I - Linux基础
* [第1章 linux基础操作](./linux-base/Readme.md)
  * [1. linux-lvm卷扩容与缩容](./linux-base/linux-lvm-extend.md)
  * [2. CentOS7 显示中文](./linux-base/Centos7-show-chinese.md)
* [第2章 linux基础命令](./linux-command/Readme.md)
  * Linux性能
    * [pidstat](./linux-command/performance/pidstat.md)
    * [mpstat](./linux-command/performance/mpstat.md)
    * [top](./linux-command/performance/top.md)
    * [dstat](./linux-command/performance/dstat.md)
    * [vmstat](./linux-command/performance/vmstat.md)
    * [strace](./linux-command/performance/strace.md)
    * [pstree](./linux-command/performance/pstree.md)
    * [sar](./linux-command/performance/sar.md)
    * [ps](./linux-command/performance/ps.md)
  * 文件和目录管理
    * [ls]
    * [tree]
    * [cat]
    * [xargs]
    * [cp]
    * [mv]
    * [rm]
    * [rmdir]
    * [管道|]
    * [find]
    * [diff]
  * 文本处理
    * [echo]
    * [head]
    * [tail]
    * [seq]
    * [sort]
    * [wc]
    * [cut]
    * [less]
    * [more]
    * [egrep]
    * [split]
    * [wc]
    * [uniq]
    * [paster]
    * [tr]
  * 文件权限属性设置
    * [umask]
    * [chmod]
    * [chgrp]
    * [chown]
    * [stat]
    * [file]
    * [lsattr]
    * [chattr]
    * [dos2unix]
  * 三剑客
    * [sed]
    * [awk] 
    * [grep]
  * 网络通讯
    * [netstat]
    * [ss]
    * [ssh]
    * [ip]
    * [route]
    * [iptable] 
    * [dig]
    * [nslookup]
    * [tcpdump]
  * 文件传输
    * [tftp]
    * [curl]
    * [scp] 
  * 备份压缩
    * [zipinfo]
    * [zip]
    * [gzip]
    * [unzip]
    * [tar]
  * 系统命令
    * [date] 

## Part II - Linux进价
* [第3章 linux网络](./linux-network/Readme.md)
* [第4章 linux故障排查](./linux-troubleshooting/Readme.md)

## Part III - Linux性能与优化
* [第5章 CPU](./linux-performance-optimizing/cpu/Readme.md)
  * [1. 基础-如何理解平均负载](./linux-performance-optimizing/cpu/understand-load-average.md) 
  * [2. 基础-理解CPU上下文](./linux-performance-optimizing/cpu/understand-cpu-context-switch.md) 
  * [3. 基础-CPU利用率达到100%](./linux-performance-optimizing/cpu/how-to-analyze-application-cpu.md) 
  * [4. 案例-CPU利用率很高,但是找不到高CPU的应用](./linux-performance-optimizing/cpu/how-to-analyze-system-cpu-02.md) 
  * [5. 案例-D进程和僵尸进程很多](./linux-performance-optimizing/cpu/how-to-deal-with-d-and-z-process.md)
  * [6. linux系统进程状态详解](./linux-performance-optimizing/cpu/understand-process-status.md)
  * [7. linux软中断CPU使用率高](./linux-performance-optimizing/cpu/linux-softinterupt.md)
  * [8. 如何迅速分析出系统CPU的瓶颈在哪里](./linux-performance-optimizing/cpu/how-to-analyze-cpu-plateau.md)
  * [9. CPU性能优化的几个思路](./linux-performance-optimizing/cpu/linux-cpu-performance-optimize.md)
* [第6章 IO](./linux-performance-optimizing/io/Readme.md)
  * [1. 理解Linux文件系统](./linux-performance-optimizing/io/understand-linux-filesystem.md)
  * [2. Linux-I/O原理](./linux-performance-optimizing/io/linux-IO-Concept.md)
  * [3. 案例-如何找出狂打日志的文件](./linux-performance-optimizing/io/find-logging-app.md)
  * [4. 案例-SQL查询很慢](./linux-performance-optimizing/io/mysql-slow.md)
  * [5. 案例-Redis响应严重延迟](./linux-performance-optimizing/io/redis-slow.md)
  * [6. 套路-如何迅速分析出系统I/O的瓶颈在哪里?](./linux-performance-optimizing/io/how-to-find-io-performance.md)

* [第7章 Memory](./linux-performance-optimizing/memory/Readme.md)
  * [1. 基础-理解Linux内存如何工作](./linux-performance-optimizing/memory/understand-linux-memory.md) 
  * [2. 案例-如何处理内存泄漏](./linux-performance-optimizing/memory/how-to-cope-with-memory-leak.md)
  * [3. swap原理](./linux-performance-optimizing/memory/understand-swap.md)
  * [4. 如何快速找出系统内存问题](./linux-performance-optimizing/memory/how-to-find-memory-question-quickly.md)
* [第8章 Network](./linux-performance-optimizing/network/Readme.md)
  * [1. 理解linux网络](linux-performance-optimizing/network/understand-linux-network.md)
  * [2. 网络IO模型](linux-performance-optimizing/network/IO-Model.md)
  * [3. 网络调试工具](linux-performance-optimizing/network/Netowk-tool.md)
  
## Part IV - 云计算
* [第9章 Openstack](./cloud/openstack/Readme.md)
  * [1. centos7 kvm硬盘在线扩容](./cloud/openstack/centos7-kvm-disk-hot-extend.md)
* [第10章 Docker容器技术](./cloud/docker/Readme.md)
  * [1. calico网络](./cloud/docker/calico.md)
  * [2. containerd](./cloud/docker/understand-containerd.md)
  * [3. kubelet-cri](./cloud/docker/understand-kubelet-cri-oci.md)



