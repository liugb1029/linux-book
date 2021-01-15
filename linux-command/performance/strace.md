## strace是什么？
按照strace官网的描述, strace是一个可用于诊断、调试和教学的Linux用户空间跟踪器。我们用它来监控用户空间进程和内核的交互，比如系统调用、信号传递、进程状态变更等。

strace底层使用内核的ptrace特性来实现其功能。

在运维的日常工作中，故障处理和问题诊断是个主要的内容，也是必备的技能。strace作为一种动态跟踪工具，能够帮助运维高效地定位进程和服务故障。它像是一个侦探，通过系统调用的蛛丝马迹，告诉你异常的真相。

## 场景
1. 在操作系统运维中会出现程序或系统命令运行失败，通过报错和日志无法定位问题根因。
2. 如何在没有内核或程序代码的情况下查看系统调用的过程。

## 说明
1. strace是有用的诊断，说明和调试工具，Linux系统管理员可以在不需要源代码的情况下即可跟踪系统的调用。
2. strace显示有关进程的系统调用的信息，这可以帮助确定一个程序使用的哪个函数，当然在系统出现问题时可以使用 strace定位系统调用过程中失败的原因，这是定位系统问题的很好的方法。

## strace 常用参数
- -c 统计每一系统调用的所执行的时间,次数和出错的次数等。
- -d 显示有关标准错误的strace本身的一些调试输出。
- -f 跟踪子进程，这些子进程是由于fork（2）系统调用而由当前跟踪的进程创建的。
- -i 在系统调用时打印指令指针。
- -t 跟踪的每一行都以时间为前缀。
- -tt 如果给出两次，则打印时间将包括微秒。
- -ttt 如果给定三次，则打印时间将包括微秒，并且前导部分将打印为自该**以来的秒数。
- -T 显示花费在系统调用上的时间。这将记录每个系统调用的开始和结束之间的时间差。
- -r 展示系统调用之间的相对时间戳。
- -v 打印环境，统计信息，termios等调用的未缩写版本。这些结构在调用中非常常见，因此默认行为显示了结构成员的合理子集。使用此选项可获取所有详细信息。
- -V 打印strace的版本号。
- -e expr 限定表达式，用于修改要跟踪的事件或如何跟踪它们：
   -e trace=set 仅跟踪指定的系统调用集。该-c选项用于确定哪些系统调用可能是跟踪有用有用。例如，trace=open，close，read，write表示仅跟踪这四个系统调用。
   -e trace=file 跟踪所有以文件名作为参数的系统调用。
   -e trace=process 跟踪涉及过程管理的所有系统调用。这对于观察进程的派生，等待和执行步骤很有用。
   -e trace=network 跟踪所有与网络相关的系统调用。
   -e trace=signal 跟踪所有与信号相关的系统调用。
   -e trace=ipc 跟踪所有与IPC相关的系统调用。
- -o 文件名 将跟踪输出写入文件名而不是stderr。
- -p pid  使用进程ID pid附加到该进程并开始跟踪。跟踪可以随时通过键盘中断信号（CTRL -C）终止。
- -S 按指定条件对-c选项打印的直方图输出进行排序。

## 示例
### 1. 打印执行uptime时系统系统调用的时间、次数、出错次数和syscall
```bash
[root@kube-master-1 ~]$ strace -c uptime
 14:33:56 up 9 days, 20:16,  1 user,  load average: 1.75, 3.37, 3.78
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 22.48    0.000553           8        66           mmap
 17.44    0.000429          10        44           mprotect
 14.19    0.000349          10        36         6 open
 10.33    0.000254           7        36           read
  8.21    0.000202           7        28           fstat
  7.28    0.000179           6        30           close
  4.31    0.000106           6        18           alarm
  3.58    0.000088           6        14           rt_sigaction
  3.46    0.000085           7        12           fcntl
  2.24    0.000055           9         6           munmap
  1.30    0.000032           6         5         4 stat
  1.10    0.000027           7         4         3 access
  1.02    0.000025           6         4           lseek
  0.69    0.000017           9         2         2 statfs
  0.53    0.000013           4         3           brk
  0.45    0.000011          11         1           write
  0.24    0.000006           6         1           rt_sigprocmask
  0.24    0.000006           6         1           uname
  0.24    0.000006           6         1           getrlimit
  0.24    0.000006           6         1           arch_prctl
  0.24    0.000006           6         1           set_robust_list
  0.20    0.000005           5         1           set_tid_address
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00    0.002460                   316        15 total
```
### 2. 打印执行uname系统调用中calls的次数排序
```bash
[root@kube-master-1 ~]$ strace -fc -S calls uname
Linux
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 35.96    0.000082          10         8           mmap
  9.21    0.000021           5         4           close
 14.47    0.000033           8         4           mprotect
 13.60    0.000031           8         4           brk
  3.95    0.000009           3         3           fstat
  3.51    0.000008           4         2           open
  8.33    0.000019          10         2           munmap
  4.39    0.000010          10         1           read
  3.51    0.000008           8         1           write
  0.00    0.000000           0         1         1 access
  0.00    0.000000           0         1           execve
  1.32    0.000003           3         1           uname
  1.75    0.000004           4         1           arch_prctl
------ ----------- ----------- --------- --------- ----------------
100.00    0.000228                    33         1 total
[root@kube-master-1 ~]$
```

### 3. 跟踪nginx，看其定时都访问了哪些文件
```bash
strace -tt -T -f -e trace=file -o /data/log/strace.log -s 1024 ./nginx
```


### 4. 系统调用的相对时间
```bash
[root@master ~]# strace -r ls
     0.000000 execve("/usr/bin/ls", ["ls"], 0x7ffe80532c38 /* 23 vars */) = 0
     0.000914 brk(NULL)                 = 0x2239000
     0.000350 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f99c809f000
     0.000160 access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
     0.000268 open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
     0.000183 fstat(3, {st_mode=S_IFREG|0644, st_size=19831, ...}) = 0
     0.000233 mmap(NULL, 19831, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f99c809a000
     0.000261 close(3)                  = 0
     0.000136 open("/lib64/libselinux.so.1", O_RDONLY|O_CLOEXEC) = 3
     0.000197 read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220j\0\0\0\0\0\0"..., 832) = 832
     0.000182 fstat(3, {st_mode=S_IFREG|0755, st_size=155744, ...}) = 0
     0.000187 mmap(NULL, 2255216, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f99c7c58000
     0.000156 mprotect(0x7f99c7c7c000, 2093056, PROT_NONE) = 0
     0.000081 mmap(0x7f99c7e7b000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x23000) = 0x7f99c7e7b000
     0.000319 mmap(0x7f99c7e7d000, 6512, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f99c7e7d000
     0.000219 close(3)                  = 0
     0.000132 open("/lib64/libcap.so.2", O_RDONLY|O_CLOEXEC) = 3
     0.000140 read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \26\0\0\0\0\0\0"..., 832) = 832
     0.000202 fstat(3, {st_mode=S_IFREG|0755, st_size=20032, ...}) = 0
     0.000160 mmap(NULL, 2114112, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f99c7a53000
```

### 5. 展示微秒级的时间戳
```bash
[root@master ~]# strace -ttt ls
1606979176.543529 execve("/usr/bin/ls", ["ls"], 0x7fff19e7d5e8 /* 23 vars */) = 0
1606979176.544371 brk(NULL)             = 0x1bdf000
1606979176.544676 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f26c9f66000
1606979176.545004 access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
1606979176.545598 open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
1606979176.545971 fstat(3, {st_mode=S_IFREG|0644, st_size=19831, ...}) = 0
1606979176.546449 mmap(NULL, 19831, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f26c9f61000
1606979176.546812 close(3)              = 0
1606979176.547556 open("/lib64/libselinux.so.1", O_RDONLY|O_CLOEXEC) = 3
1606979176.548221 read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220j\0\0\0\0\0\0"..., 832) = 832
1606979176.548598 fstat(3, {st_mode=S_IFREG|0755, st_size=155744, ...}) = 0
1606979176.548816 mmap(NULL, 2255216, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f26c9b1f000
1606979176.549198 mprotect(0x7f26c9b43000, 2093056, PROT_NONE) = 0
1606979176.549260 mmap(0x7f26c9d42000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x23000) = 0x7f26c9d42000
1606979176.549363 mmap(0x7f26c9d44000, 6512, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f26c9d44000
1606979176.549559 close(3)              = 0
1606979176.549676 open("/lib64/libcap.so.2", O_RDONLY|O_CLOEXEC) = 3
```

### 6. 保存输出到文件
使用-o选项可以把strace命令输出的结果保存到一个文件中。
```bash
[root@master ~]# strace -o process_strace -p 867
strace: Process 867 attached
^Cstrace: Process 867 detached
[root@master ~]# cat process_strace
restart_syscall(<... resuming interrupted read ...>) = -1 ETIMEDOUT (Connection timed out)
futex(0x55819a83ba70, FUTEX_WAKE, 1)    = 1
futex(0xc420095948, FUTEX_WAKE, 1)      = 1
futex(0x55819a83f940, FUTEX_WAIT, 0, {tv_sec=86, tv_nsec=412337901}) = 0
futex(0x55819a83f940, FUTEX_WAIT, 0, {tv_sec=2, tv_nsec=999812080}) = 0
futex(0x55819a83ba70, FUTEX_WAKE, 1)    = 1
futex(0xc42005dd48, FUTEX_WAKE, 1)      = 1
futex(0x55819a83f940, FUTEX_WAIT, 0, {tv_sec=0, tv_nsec=4998
```

## 7、查看进程发起的系统调用
strace跟踪进程的系统调用，-f 表示跟踪子进程和子线程，-T 表示显示系统调用的时长，-tt 表示显示跟踪时间：
```bash
$ strace -f -T -tt -p 9085
[pid  9085] 14:20:16.826131 epoll_pwait(5, [{EPOLLIN, {u32=8, u64=8}}], 10128, 65, NULL, 8) = 1 <0.000055>
[pid  9085] 14:20:16.826301 read(8, "*2\r\n$3\r\nGET\r\n$41\r\nuuid:5b2e76cc-"..., 16384) = 61 <0.000071>
[pid  9085] 14:20:16.826477 read(3, 0x7fff366a5747, 1) = -1 EAGAIN (Resource temporarily unavailable) <0.000063>
[pid  9085] 14:20:16.826645 write(8, "$3\r\nbad\r\n", 9) = 9 <0.000173>
[pid  9085] 14:20:16.826907 epoll_pwait(5, [{EPOLLIN, {u32=8, u64=8}}], 10128, 65, NULL, 8) = 1 <0.000032>
[pid  9085] 14:20:16.827030 read(8, "*2\r\n$3\r\nGET\r\n$41\r\nuuid:55862ada-"..., 16384) = 61 <0.000044>
[pid  9085] 14:20:16.827149 read(3, 0x7fff366a5747, 1) = -1 EAGAIN (Resource temporarily unavailable) <0.000043>
[pid  9085] 14:20:16.827285 write(8, "$3\r\nbad\r\n", 9) = 9 <0.000141>
[pid  9085] 14:20:16.827514 epoll_pwait(5, [{EPOLLIN, {u32=8, u64=8}}], 10128, 64, NULL, 8) = 1 <0.000049>
[pid  9085] 14:20:16.827641 read(8, "*2\r\n$3\r\nGET\r\n$41\r\nuuid:53522908-"..., 16384) = 61 <0.000043>
[pid  9085] 14:20:16.827784 read(3, 0x7fff366a5747, 1) = -1 EAGAIN (Resource temporarily unavailable) <0.000034>
[pid  9085] 14:20:16.827945 write(8, "$4\r\ngood\r\n", 10) = 10 <0.000288>
[pid  9085] 14:20:16.828339 epoll_pwait(5, [{EPOLLIN, {u32=8, u64=8}}], 10128, 63, NULL, 8) = 1 <0.000057>
[pid  9085] 14:20:16.828486 read(8, "*3\r\n$4\r\nSADD\r\n$4\r\ngood\r\n$36\r\n535"..., 16384) = 67 <0.000040>
[pid  9085] 14:20:16.828623 read(3, 0x7fff366a5747, 1) = -1 EAGAIN (Resource temporarily unavailable) <0.000052>
[pid  9085] 14:20:16.828760 write(7, "*3\r\n$4\r\nSADD\r\n$4\r\ngood\r\n$36\r\n535"..., 67) = 67 <0.000060>
[pid  9085] 14:20:16.828970 fdatasync(7) = 0 <0.005415>
[pid  9085] 14:20:16.834493 write(8, ":1\r\n", 4) = 4 <0.000250>
```