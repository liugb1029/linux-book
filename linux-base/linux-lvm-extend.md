在工作中遇到根目录容量不够，于是结合网上这篇文章，把虚拟机里扩展的十几个GB的容量扩展到根目录下

# CentOS7，LVM根分区扩容步骤

1.查看现有分区大小

df -TH

![](/image/chapter1/20191022124405358.png)

LVM分区，磁盘总大小为20G,根分区总容量为17G  
2.关机增加大小为30G\(测试环境使用的Vmware Workstation\)

![](/image/chapter1/20191022124420983.png)  
扩展分区到30G  
3.查看扩容后磁盘大小

df -TH

lsblk  
![](/image/chapter1/20191022124435221.png)

磁盘总大小为30G,根分区为17G  
4.创建分区

fdisk /dev/sda  
![](/image/chapter1/20191022124451240.png)

将sda剩余空间全部给sda3  
5.刷新分区并创建物理卷

partprobe /dev/sda

pvcreate /dev/sda3  
![](/image/chapter1/20191022124515659.png)

6.查看卷组名称，以及卷组使用情况

vgdisplay  
![](/image/chapter1/20191022124529172.png)

VG Name为centos  
7.将物理卷扩展到卷组

vgextend centos /dev/sda3  
![](/image/chapter1/20191022124543929.png)

使用sda3扩展VG centos  
8.查看当前逻辑卷的空间状态

lvdisplay  
![](/image/chapter1/20191022124557124.png)

需要扩展LV /dev/centos/root  
9.将卷组中的空闲空间扩展到根分区逻辑卷

lvextend -l +100%FREE /dev/centos/root  
![](/image/chapter1/20191022124613329.png)

10.刷新根分区

xfs\_growfs /dev/centos/root  
![](/image/chapter1/20191022124626320.png)

11.查看磁盘使用情况，扩展之前和之后是不一样的  
![](/image/chapter1/20191022124636364.png)

根分区已经变成27G

# CentOS7,非LVM根分区扩容步骤：

1.查看现有的分区大小  
![](/image/chapter1/20191022124810280.png)

非LVM分区，目前磁盘大小为20G，根分区总容量为17G  
2.关机增加磁盘大小为30G  
![](/image/chapter1/201910221248184.png)

3.查看磁盘扩容后状态

lsblk

dh -TH  
![](/image/chapter1/20191022124841683.png)

现在磁盘总大小为30G,根分区为17G  
4.进行分区扩展磁盘，记住根分区起始位置和结束位置  
![](/image/chapter1/20191022124841683.png)

5.删除根分区，切记不要保存  
![](/image/chapter1/20191022124850554.png)

6.创建分区，箭头位置为分区起始位置  
![](/image/chapter1/20191022124859451.png)

7.保存退出并刷新分区

partpeobe /dev/sda  
![](/image/chapter1/20191022124912144.png)

8.查看分区状态  
![](/image/chapter1/20191022124919891.png)

这里不知道为啥变成19G了。。  
9.刷新根分区并查看状态

xfs\_growfs /dev/sda3  
![](/image/chapter1/20191022124929242.png)

