## 复习
文件系统，是对存储设备上的文件进行组织管理的一种机制。为了支持各类不同的文件系统，Linux 在各种文件系统上，抽象了一层虚拟文件系统 VFS。

它定义了一组所有文件系统都支持的数据结构和标准接口。这样，应用程序和内核中的其他子系统，就只需要跟 VFS 提供的统一接口进行交互。

在文件系统的下层，为了支持各种不同类型的存储设备，Linux 又在各种存储设备的基础上，抽象了一个通用块层。

通用块层，为文件系统和应用程序提供了访问块设备的标准接口；同时，为各种块设备的驱动程序提供了统一的框架。此外，通用块层还会对文件系统和应用程序发送过来的 I/O 请求进行排队，并通过重新排序、请求合并等方式，提高磁盘读写的效率。

通用块层的下一层，自然就是设备层了，包括各种块设备的驱动程序以及物理存储设备。

文件系统、通用块层以及设备层，就构成了 Linux 的存储 I/O 栈。存储系统的 I/O ，通常是整个系统中最慢的一环。所以，Linux 采用多种缓存机制，来优化 I/O 的效率，比方说，

- 为了优化文件访问的性能，采用页缓存、索引节点缓存、目录项缓存等多种缓存机制，减少对下层块设备的直接调用。
- 同样的，为了优化块设备的访问效率，使用缓冲区来缓存块设备的数据。

## 案例分析

首先，我们在终端中执行下面的命令，运行今天的目标应用：
```bash
$ docker run -v /tmp:/tmp --name=app -itd feisky/logapp
```
然后，在终端中运行 ps 命令，确认案例应用正常启动。如果操作无误，你应该可以在 ps 的输出中，看到一个 app.py 的进程：
```bash
$ ps -ef | grep /app.py 
root     18940 18921 73 14:41 pts/0    00:00:02 python /app.py 
```

接着，我们来看看系统有没有性能问题。要观察哪些性能指标呢？前面文章中，我们知道 CPU、内存和磁盘 I/O 等系统资源，很容易出现资源瓶颈，这就是我们观察的方向了。我们来观察一下这些资源的使用情况。

当然，动手之前你应该想清楚，要用哪些工具来做，以及工具的使用顺序又是怎样的。你可以先回忆下前面的案例和思路，自己想一想，然后再继续下面的步骤。

我的想法是，我们可以先用 top ，来观察 CPU 和内存的使用情况；然后再用 iostat ，来观察磁盘的 I/O 情况。

所以，接下来，你可以在终端中运行 top 命令，观察 CPU 和内存的使用情况：
```bash
# 按1切换到每个CPU的使用情况 
$ top 
top - 14:43:43 up 1 day,  1:39,  2 users,  load average: 2.48, 1.09, 0.63 
Tasks: 130 total,   2 running,  74 sleeping,   0 stopped,   0 zombie 
%Cpu0  :  0.7 us,  6.0 sy,  0.0 ni,  0.7 id, 92.7 wa,  0.0 hi,  0.0 si,  0.0 st 
%Cpu1  :  0.0 us,  0.3 sy,  0.0 ni, 92.3 id,  7.3 wa,  0.0 hi,  0.0 si,  0.0 st 
KiB Mem :  8169308 total,   747684 free,   741336 used,  6680288 buff/cache 
KiB Swap:        0 total,        0 free,        0 used.  7113124 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND 
18940 root      20   0  656108 355740   5236 R   6.3  4.4   0:12.56 python 
1312 root      20   0  236532  24116   9648 S   0.3  0.3   9:29.80 python3 
```

观察 top 的输出，你会发现，CPU0 的使用率非常高，它的系统 CPU 使用率（sys%）为 6%，而 iowait 超过了 90%。这说明 CPU0 上，可能正在运行 I/O 密集型的进程。不过，究竟是什么原因呢？这个疑问先保留着，我们先继续看完。

接着我们来看，进程部分的 CPU 使用情况。你会发现， python 进程的 CPU 使用率已经达到了 6%，而其余进程的 CPU 使用率都比较低，不超过 0.3%。看起来 python 是个可疑进程。记下 python 进程的 PID 号 18940，我们稍后分析。

最后再看内存的使用情况，总内存 8G，剩余内存只有 730 MB，而 Buffer/Cache 占用内存高达 6GB 之多，这说明内存主要被缓存占用。虽然大部分缓存可回收，我们还是得了解下缓存的去处，确认缓存使用都是合理的。

到这一步，你基本可以判断出，CPU 使用率中的 iowait 是一个潜在瓶颈，而内存部分的缓存占比较大，那磁盘 I/O 又是怎么样的情况呢？

我们在终端中按 Ctrl+C ，停止 top 命令，再运行 iostat 命令，观察 I/O 的使用情况：
```bash
# -d表示显示I/O性能指标，-x表示显示扩展统计（即所有I/O指标） 
$ iostat -x -d 1 
Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util 
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00 
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00 
sda              0.00   64.00      0.00  32768.00     0.00     0.00   0.00   0.00    0.00 7270.44 1102.18     0.00   512.00  15.50  99.20
```

还记得这些性能指标的含义吗？先自己回忆一下，如果实在想不起来，查看上一节的内容，或者用 man iostat 查询。

观察 iostat 的最后一列，你会看到，磁盘 sda 的 I/O 使用率已经高达 99%，很可能已经接近 I/O 饱和。

再看前面的各个指标，每秒写磁盘请求数是 64 ，写大小是 32 MB，写请求的响应时间为 7 秒，而请求队列长度则达到了 1100。

超慢的响应时间和特长的请求队列长度，进一步验证了 I/O 已经饱和的猜想。此时，sda 磁盘已经遇到了严重的性能瓶颈。

到这里，也就可以理解，为什么前面看到的 iowait 高达 90% 了，这正是磁盘 sda 的 I/O 瓶颈导致的。接下来的重点就是分析 I/O 性能瓶颈的根源了。那要怎么知道，这些 I/O 请求相关的进程呢？

不知道你还记不记得，上一节我曾提到过，可以用 pidstat 或者 iotop ，观察进程的 I/O 情况。这里，我就用 pidstat 来看一下。

使用 pidstat 加上 -d 参数，就可以显示每个进程的 I/O 情况。所以，你可以在终端中运行如下命令来观察：
```bash
$ pidstat -d 1 

15:08:35      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command 
15:08:36        0     18940      0.00  45816.00      0.00      96  python 

15:08:36      UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s iodelay  Command 
15:08:37        0       354      0.00      0.00      0.00     350  jbd2/sda1-8 
15:08:37        0     18940      0.00  46000.00      0.00      96  python 
15:08:37        0     20065      0.00      0.00      0.00    1503  kworker/u4:2 
```
从 pidstat 的输出，你可以发现，只有 python 进程的写比较大，而且每秒写的数据超过 45 MB，比上面 iostat 发现的 32MB 的结果还要大。很明显，正是 python 进程导致了 I/O 瓶颈。

再往下看 iodelay 项。虽然只有 python 在大量写数据，但你应该注意到了，有两个进程 （kworker 和 jbd2 ）的延迟，居然比 python 进程还大很多。

这其中，kworker 是一个内核线程，而 jbd2 是 ext4 文件系统中，用来保证数据完整性的内核线程。他们都是保证文件系统基本功能的内核线程，所以具体细节暂时就不用管了，我们只需要明白，它们延迟的根源还是大量 I/O。

综合 pidstat 的输出来看，还是 python 进程的嫌疑最大。接下来，我们来分析 python 进程到底在写什么。

首先留意一下 python 进程的 PID 号， 18940。看到 18940 ，你有没有觉得熟悉？其实前面在使用 top 时，我们记录过的 CPU 使用率最高的进程，也正是它。不过，虽然在 top 中使用率最高，也不过是 6%，并不算高。所以，以 I/O 问题为分析方向还是正确的。

知道了进程的 PID 号，具体要怎么查看写的情况呢？

其实，我在系统调用的案例中讲过，读写文件必须通过系统调用完成。观察系统调用情况，就可以知道进程正在写的文件。想起 strace 了吗，它正是我们分析系统调用时最常用的工具。

接下来，我们在终端中运行 strace 命令，并通过 -p 18940 指定 python 进程的 PID 号：
```bash
$ strace -p 18940 
strace: Process 18940 attached 
...
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0f7aee9000 
mmap(NULL, 314576896, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f0f682e8000 
write(3, "2018-12-05 15:23:01,709 - __main"..., 314572844 
) = 314572844 
munmap(0x7f0f682e8000, 314576896)       = 0 
write(3, "\n", 1)                       = 1 
munmap(0x7f0f7aee9000, 314576896)       = 0 
close(3)                                = 0 
stat("/tmp/logtest.txt.1", {st_mode=S_IFREG|0644, st_size=943718535, ...}) = 0 
```
从 write() 系统调用上，我们可以看到，进程向文件描述符编号为 3 的文件中，写入了 300MB 的数据。看来，它应该是我们要找的文件。不过，write() 调用中只能看到文件的描述符编号，文件名和路径还是未知的。

再观察后面的 stat() 调用，你可以看到，它正在获取 /tmp/logtest.txt.1 的状态。 这种“点 + 数字格式”的文件，在日志回滚中非常常见。我们可以猜测，这是第一个日志回滚文件，而正在写的日志文件路径，则是 /tmp/logtest.txt。

当然，这只是我们的猜测，自然还需要验证。这里，我再给你介绍一个新的工具 lsof。它专门用来查看进程打开文件列表，不过，这里的“文件”不只有普通文件，还包括了目录、块设备、动态库、网络套接字等。

接下来，我们在终端中运行下面的 lsof 命令，看看进程 18940 都打开了哪些文件：
```bash
$ lsof -p 18940 
COMMAND   PID USER   FD   TYPE DEVICE  SIZE/OFF    NODE NAME 
python  18940 root  cwd    DIR   0,50      4096 1549389 / 
python  18940 root  rtd    DIR   0,50      4096 1549389 / 
… 
python  18940 root    2u   CHR  136,0       0t0       3 /dev/pts/0 
python  18940 root    3w   REG    8,1 117944320     303 /tmp/logtest.txt 
```
这个输出界面中，有几列我简单介绍一下，FD 表示文件描述符号，TYPE 表示文件类型，NAME 表示文件路径。这也是我们需要关注的重点。

再看最后一行，这说明，这个进程打开了文件 /tmp/logtest.txt，并且它的文件描述符是 3 号，而 3 后面的 w ，表示以写的方式打开。

这跟刚才 strace 完我们猜测的结果一致，看来这就是问题的根源：进程 18940 以每次 300MB 的速度，在“疯狂”写日志，而日志文件的路径是 /tmp/logtest.txt。

既然找出了问题根源，接下来按照惯例，就该查看源代码，然后分析为什么这个进程会狂打日志了。

你可以运行 docker cp 命令，把案例应用的源代码拷贝出来，然后查看它的内容。（你也可以点击这里查看案例应用的源码）：
```bash
#拷贝案例应用源代码到当前目录
$ docker cp app:/app.py . 

#查看案例应用的源代码
$ cat app.py 

logger = logging.getLogger(__name__) 
logger.setLevel(level=logging.INFO) 
rHandler = RotatingFileHandler("/tmp/logtest.txt", maxBytes=1024 * 1024 * 1024, backupCount=1) 
rHandler.setLevel(logging.INFO) 

def write_log(size): 
  '''Write logs to file''' 
  message = get_message(size) 
  while True: 
    logger.info(message) 
    time.sleep(0.1) 

if __name__ == '__main__': 
  msg_size = 300 * 1024 * 1024 
  write_log(msg_size) 
```
分析这个源码，我们发现，它的日志路径是 /tmp/logtest.txt，默认记录 INFO 级别以上的所有日志，而且每次写日志的大小是 300MB。这跟我们上面的分析结果是一致的。

一般来说，生产系统的应用程序，应该有动态调整日志级别的功能。继续查看源码，你会发现，这个程序也可以调整日志级别。如果你给它发送 SIGUSR1 信号，就可以把日志调整为 INFO 级；发送 SIGUSR2 信号，则会调整为 WARNING 级：

```bash
def set_logging_info(signal_num, frame): 
  '''Set loging level to INFO when receives SIGUSR1''' 
  logger.setLevel(logging.INFO) 

def set_logging_warning(signal_num, frame): 
  '''Set loging level to WARNING when receives SIGUSR2''' 
  logger.setLevel(logging.WARNING) 

signal.signal(signal.SIGUSR1, set_logging_info) 
signal.signal(signal.SIGUSR2, set_logging_warning) 
```
根据源码中的日志调用 logger. info(message) ，我们知道，它的日志是 INFO 级，这也正是它的默认级别。那么，只要把默认级别调高到 WARNING 级，日志问题应该就解决了。

接下来，我们就来检查一下，刚刚的分析对不对。在终端中运行下面的 kill 命令，给进程 18940 发送 SIGUSR2 信号：
```bash
$ kill -SIGUSR2 18940 
```
然后，再执行 top 和 iostat 观察一下：
```bash
$ top 
... 
%Cpu(s):  0.3 us,  0.2 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st 

$ iostat -d -x 1
Device            r/s     w/s     rkB/s     wkB/s   rrqm/s   wrqm/s  %rrqm  %wrqm r_await w_await aqu-sz rareq-sz wareq-sz  svctm  %util 
loop0            0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00 
sdb              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00 
sda              0.00    0.00      0.00      0.00     0.00     0.00   0.00   0.00    0.00    0.00   0.00     0.00     0.00   0.00   0.00
```
