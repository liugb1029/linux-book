# [Centos7 KVM虚拟机硬盘热添加和扩容](https://www.cnblogs.com/aqicheng/p/13042604.html)

一、**为虚拟机添加一块数据盘**

1、添加一块磁盘

```bash
qemu-img create -f qcow2 web-add.qcow2 2G
```

2、虚拟机添加一块硬盘

```rust
virsh attach-disk web /opt/web-add.qcow2 vdb --subdriver qcow2　　　　　　    临时添加磁盘


virsh attach-disk web /opt/web-add.qcow2 vdb --subdriver qcow2   --config   永久添加磁盘


virsh detach-disk web vdb                                                   分离磁盘
```

3、格式化硬盘，并挂载硬盘

```
lsblk                                                　　　　　　　　　　　　　　查看添加硬盘是否添加成功
```

**二、把数据盘扩容**

1、首先卸载硬盘

```
umount /mnt
```

2、分离磁盘

```
virsh detach-disk web vdb
```

3、调整磁盘为4G

```
qemu-img resize web-add.qcow2 4G
```

![](https://img2020.cnblogs.com/blog/1205182/202006/1205182-20200604112017005-1739757834.png)

4、给虚拟机添加硬盘

```
virsh attach-disk web /opt/web-add.qcow2 vdb --subdriver qcow2
```

5、查看硬盘情况

```
[root@localhost ~]# mount /dev/vdb /mnt                                             挂载硬盘
[root@localhost ~]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root  3.5G  981M  2.6G   28% /
devtmpfs                 484M     0  484M    0% /dev
tmpfs                    496M     0  496M    0% /dev/shm
tmpfs                    496M  6.7M  490M    2% /run
tmpfs                    496M     0  496M    0% /sys/fs/cgroup
/dev/vda1               1014M  130M  885M   13% /boot
tmpfs                    100M     0  100M    0% /run/user/0
/dev/vdb                 2.0G   33M  2.0G    2% /mnt                                 分区情况没有变，需要手动修改
[root@localhost ~]# fdisk -l

磁盘 /dev/vda：5368 MB, 5368709120 字节，10485760 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x000ce86b

   设备 Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048     2099199     1048576   83  Linux
/dev/vda2         2099200    10485759     4193280   8e  Linux LVM

磁盘 /dev/mapper/centos-root：3753 MB, 3753902080 字节，7331840 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节

磁盘 /dev/mapper/centos-swap：536 MB, 536870912 字节，1048576 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节

磁盘 /dev/vdb：4294 MB, 4294967296 字节，8388608 个扇区                                   这里分区变大了
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
```

6.手动修改分区情况

```
[root@localhost ~]# xfs_growfs /mnt
meta-data=/dev/vdb               isize=512    agcount=4, agsize=131072 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=524288, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 524288 to 1048576                                              这里变化了

[root@localhost ~]# df -h
文件系统 容量 已用 可用 已用% 挂载点
/dev/mapper/centos-root 3.5G 981M 2.6G 28% /
devtmpfs 484M 0 484M 0% /dev
tmpfs 496M 0 496M 0% /dev/shm
tmpfs 496M 6.7M 490M 2% /run
tmpfs 496M 0 496M 0% /sys/fs/cgroup
/dev/vda1 1014M 130M 885M 13% /boot
tmpfs 100M 0 100M 0% /run/user/0
/dev/vdb 4.0G 33M 4.0G 1% /mnt                                                           已经变成4G了
```

**三、系统盘扩容**

**　　注意：此功能有前提：安装系统的时候选择自定义分区，选择标准分区，只添加一个根目录，所有空间都给根分区，此操作才能实现。**

1、关机

```
[root@centoszhu opt]# virsh destroy web
```

2、、查看系统盘

```bash
[root@centoszhu opt]# qemu-img info web.qcow2 
image: web.qcow2
file format: qcow2
virtual size: 5.0G (5368709120 bytes)                                                   系统盘只有5个G
disk size: 1.3G
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: false
```

3、调整硬盘大小

```
[root@centoszhu opt]# qemu-img resize web.qcow2 10G
[root@centoszhu opt]# qemu-img info web.qcow2 
image: web.qcow2
file format: qcow2
virtual size: 10G (10737418240 bytes)
disk size: 1.3G
cluster_size: 65536
Format specific information:
compat: 1.1
lazy refcounts: false
[root@centoszhu opt]# virsh start web
域 web 已开始
```

4、虚拟机查看硬盘大小

```
[root@localhost ~]# df -h
文件系统 容量 已用 可用 已用% 挂载点
/dev/vda1 5.0G 1.1G 4.0G 21% /
devtmpfs 486M 0 486M 0% /dev
tmpfs 496M 0 496M 0% /dev/shm
tmpfs 496M 6.7M 490M 2% /run
tmpfs 496M 0 496M 0% /sys/fs/cgroup
tmpfs 100M 0 100M 0% /run/user/0
[root@localhost ~]# fdisk -l

磁盘 /dev/vda：5368 MB, 5368709120 字节，10485760 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x000bfe9a

设备 Boot Start End Blocks Id System
/dev/vda1 * 2048 10485759 5241856 83 Linux
```



