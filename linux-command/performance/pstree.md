## pstree简介
pstree命令以树状图显示进程间的关系（display a tree of processes）。ps命令可以显示当前正在运行的那些进程的信息，但是对于它们之间的关系却显示得不够清晰。在Linux系统中，系统调用fork可以创建子进程，通过子shell也可以创建子进程，Linux系统中进程之间的关系天生就是一棵树，树的根就是进程PID为1的init进程。


## 常用选项
- -a：显示每个程序的完整指令，包含路径，参数或是常驻服务的标示；
- -c：不合并相同的进程；
- -G：使用VT100终端机的列绘图字符；
- -h：列出树状图时，特别标明现在执行的程序；
- -H<程序识别码>：此参数的效果和指定"-h"参数类似，但特别标明指定的程序；
- -l：采用长列格式显示树状图；
- -n：用进程ID排序。预设是以程序名称来排序；
- -p：显示进程id；
- -u：显示用户名称；
- -U：使用UTF-8列绘图字符；
- -V：显示版本信息。

## 示例

### 1、默认输出(相同进程会合并显示) -c表示不合并相同的进程
```bash
# 进程前的数字就表示同一个进程的总数
[root@master ~]# pstree
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─agetty
        ├─auditd───{auditd}
        ├─crond
        ├─dbus-daemon
        ├─dockerd─┬─docker-containe─┬─docker-containe─┬─nginx───nginx
        │         │                 │                 └─9*[{docker-containe}]
        │         │                 └─11*[{docker-containe}]
        │         └─12*[{dockerd}]
        ├─irqbalance
        ├─lvmetad
        ├─master─┬─pickup
        │        └─qmgr
        ├─polkitd───5*[{polkitd}]
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd───sshd───bash───pstree
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        └─tuned───4*[{tuned}]
# -c 不合并相同的进程

[root@master ~]# pstree -c
systemd─┬─NetworkManager─┬─{NetworkManager}
        │                └─{NetworkManager}
        ├─agetty
        ├─auditd───{auditd}
        ├─crond
        ├─dbus-daemon
        ├─dockerd─┬─docker-containe─┬─docker-containe─┬─nginx───nginx
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 ├─{docker-containe}
        │         │                 │                 └─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 ├─{docker-containe}
        │         │                 └─{docker-containe}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         ├─{dockerd}
        │         └─{dockerd}
        ├─irqbalance
        ├─lvmetad
        ├─master─┬─pickup
        │        └─qmgr
        ├─polkitd─┬─{polkitd}
        │         ├─{polkitd}
        │         ├─{polkitd}
        │         ├─{polkitd}
        │         └─{polkitd}
        ├─rsyslogd─┬─{rsyslogd}
        │          └─{rsyslogd}
        ├─sshd───sshd───bash───pstree
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        └─tuned─┬─{tuned}
                ├─{tuned}
                ├─{tuned}
                └─{tuned}

```
### 2、显示进程PID
```bash
[root@master ~]# pstree -p
systemd(1)─┬─NetworkManager(650)─┬─{NetworkManager}(666)
           │                     └─{NetworkManager}(669)
           ├─agetty(660)
           ├─auditd(619)───{auditd}(620)
           ├─crond(657)
           ├─dbus-daemon(643)
           ├─dockerd(867)─┬─docker-containe(1022)─┬─docker-containe(16725)─┬─nginx(16743)───nginx(16785)
           │              │                       │                        ├─{docker-containe}(16726)
           │              │                       │                        ├─{docker-containe}(16727)
           │              │                       │                        ├─{docker-containe}(16728)
           │              │                       │                        ├─{docker-containe}(16729)
           │              │                       │                        ├─{docker-containe}(16730)
           │              │                       │                        ├─{docker-containe}(16731)
           │              │                       │                        ├─{docker-containe}(16732)
           │              │                       │                        ├─{docker-containe}(16734)
           │              │                       │                        └─{docker-containe}(16810)
           │              │                       ├─{docker-containe}(1026)
           │              │                       ├─{docker-containe}(1027)
           │              │                       ├─{docker-containe}(1028)
           │              │                       ├─{docker-containe}(1029)
           │              │                       ├─{docker-containe}(1030)
           │              │                       ├─{docker-containe}(1048)
           │              │                       ├─{docker-containe}(1055)
           │              │                       ├─{docker-containe}(1057)
           │              │                       ├─{docker-containe}(1058)
           │              │                       ├─{docker-containe}(1636)
           │              │                       └─{docker-containe}(1909)
           │              ├─{dockerd}(968)
           │              ├─{dockerd}(969)
           │              ├─{dockerd}(970)
           │              ├─{dockerd}(971)
           │              ├─{dockerd}(993)
           │              ├─{dockerd}(1012)
           │              ├─{dockerd}(1067)
           │              ├─{dockerd}(1068)
           │              ├─{dockerd}(1070)
           │              ├─{dockerd}(1071)
           │              ├─{dockerd}(1598)
           │              └─{dockerd}(1599)
           ├─irqbalance(641)
           ├─lvmetad(491)
           ├─master(1156)─┬─pickup(17014)
           │              └─qmgr(1167)
           ├─polkitd(647)─┬─{polkitd}(661)
           │              ├─{polkitd}(662)
           │              ├─{polkitd}(663)
           │              ├─{polkitd}(664)
           │              └─{polkitd}(665)
           ├─rsyslogd(865)─┬─{rsyslogd}(939)
           │               └─{rsyslogd}(940)
           ├─sshd(863)───sshd(12011)───bash(12014)───pstree(21561)
           ├─systemd-journal(469)
           ├─systemd-logind(651)
           ├─systemd-udevd(505)
           └─tuned(862)─┬─{tuned}(1257)
                        ├─{tuned}(1259)
                        ├─{tuned}(1263)
                        └─{tuned}(1358)
```
### 3、显示指定PID的进程以及子进程
```bash
[root@master ~]# pstree 16725
docker-containe─┬─nginx───nginx
                └─9*[{docker-containe}]
[root@master ~]#
[root@master ~]#
[root@master ~]#
[root@master ~]# pstree -p 16725
docker-containe(16725)─┬─nginx(16743)───nginx(16785)
                       ├─{docker-containe}(16726)
                       ├─{docker-containe}(16727)
                       ├─{docker-containe}(16728)
                       ├─{docker-containe}(16729)
                       ├─{docker-containe}(16730)
                       ├─{docker-containe}(16731)
                       ├─{docker-containe}(16732)
                       ├─{docker-containe}(16734)
                       └─{docker-containe}(16810)
[root@master ~]#

```
### 4、显示命令行参数
```
[root@master ~]# pstree -a
systemd --switched-root --system --deserialize 22
  ├─NetworkManager --no-daemon
  │   └─2*[{NetworkManager}]
  ├─agetty --noclear tty1 linux
  ├─auditd
  │   └─{auditd}
  ├─crond -n
  ├─dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
  ├─dockerd
  │   ├─docker-containe --config /var/run/docker/containerd/containerd.toml
  │   │   ├─docker-containe -namespace moby -workdir...
  │   │   │   ├─nginx
  │   │   │   │   └─nginx
  │   │   │   └─9*[{docker-containe}]
  │   │   └─11*[{docker-containe}]
  │   └─12*[{dockerd}]
  ├─irqbalance --foreground
  ├─lvmetad -f
  ├─master -w
  │   ├─pickup -l -t unix -u
  │   └─qmgr -l -t unix -u
  ├─polkitd --no-debug
  │   └─5*[{polkitd}]
  ├─rsyslogd -n
  │   └─2*[{rsyslogd}]
  ├─sshd -D
  │   └─sshd
  │       └─bash
  │           └─pstree -a
  ├─systemd-journal
  ├─systemd-logind
  ├─systemd-udevd
  └─tuned -Es /usr/sbin/tuned -l -P
      └─4*[{tuned}]
```

### 5、查看某个用户的进程
```bash
[root@master ~]# pstree -u
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─agetty
        ├─auditd───{auditd}
        ├─crond
        ├─dbus-daemon(dbus)
        ├─dockerd─┬─docker-containe─┬─docker-containe─┬─nginx───nginx(101)
        │         │                 │                 └─9*[{docker-containe}]
        │         │                 └─11*[{docker-containe}]
        │         └─12*[{dockerd}]
        ├─irqbalance
        ├─lvmetad
        ├─master─┬─pickup(postfix)
        │        └─qmgr(postfix)
        ├─polkitd(polkitd)───5*[{polkitd}]
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd───sshd───bash───pstree
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        └─tuned───4*[{tuned}]
[root@master ~]#
[root@master ~]#
[root@master ~]# pstree -p postfix
pickup(22173)

qmgr(1167)
```





