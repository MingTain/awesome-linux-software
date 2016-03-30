
# 目录

- [运维配置管理工具](#运维配置管理工具)
- [性能测试工具](#性能测试工具)
- [Memory Usage](#Memory Usage)
- [Disk IO](#Disk IO)
- [Network IO](#Network IO)
- [压力测试工具](#压力测试工具)
- [博客工具](#博客工具)
- [监控应用](#监控应用)
- [Linux终端](#Linux终端)


##  运维配置管理工具
---

|工具|简介|安装配置|星级|详情|备注|
|---|---|---|---|---|---|
|Puppet|Linux、Unix、Windows平台的集中配置管理系统|略|★★★★☆|[详情](Tools/Puppet/Puppet.md)|无|
|Ansible|GNOME下的下拉式终端|略|★★★☆☆|[详情](Tools/Ansible/Ansible.md)|无|
|Foreman|集成的数据中心生命周期管理工具|略|★★★☆☆|[详情](Tools/Foreman/Foreman.md)|无|


### 其它

Cron jobs

Subversion

Chef

SaltStack

CFEngine

NixOps

### 选择

Puppet（Puppet + Foreman）&gt; Ansible

如果需求比较简单就：Ansible &gt; Puppet（Puppet + Foreman）



## 性能测试工具
---

### 工具：[top](http://superuser.com/questions/575202/understanding-top-command-in-unix/575330#575330)

All this information is available in the **top man page** which you can read by running ``man top``. Here is a breakdown:

![top](http://gitlab.local/algorithm/frequency/uploads/d14f87b9caf852560ba6acd7661bde1c/top.png)

- The **CPU(s)** row shows:

>  CPU state percentages based on the interval since the last refresh. Where two labels are shown below, those for more recent kernel versions are shown first.

>  **us, user** : time running un-niced user processes

>  **sy, system** : time running kernel processes

>  **ni, nice** : time running niced user processes

>  **wa, IO-wait** : time waiting for I/O completion

>  **hi** : time spent servicing hardware interrupts

>  **si** : time spent servicing software interrupts

>  **st** : time stolen from this vm by the hypervisor

- The **Mem** and **Swap** row show:


>    This portion consists of two lines which may express values in kibibytes (KiB), mebibytes (MiB) or gibibytes (GiB) depending on the amount of currently installed physical memory.

>    Line 1 reflects physical memory, classified as: total, used, free, buffers

>    Line 2 reflects virtual memory, classified as: total, used, free, cached



Physical memory is your RAM, physical pieces of hardware that provide Random Access Memory. Swap is virtual memory which can be a file or a partition on your hard drive that is essentially used as extra RAM. It is not a separate RAM chip though, it resides on your hard drive.


- The last section provides information about the currently running processes. It consists of the following columns:

>   **PID** -- Process Id : This is a unique number used to identify the process.

>   **User** : The username of whoever launched the process.

>   **PR** -- Priority : The priority of the process. Processes with higher priority will be favored by the kernel and given more CPU time than processes with lower priority. ``Oddly enough, the lower this value, the higher the actual priority; the highest priority on *nix is -20 and the lowest is 20.``

>   **NI** -- Nice value : nice is a way of setting your process' priority. See [here](http://en.wikipedia.org/wiki/Nice_%28Unix%29) for more details.(注：「Nice值」这个名称来自英文单词nice，意思为友好。Nice值越高，这个进程越「友好」，就会让给其他进程越多的时间。)

>   **VIRT** -- Virtual Memory Size (KiB) : The total amount of virtual memory used by the process.

>   **RES** -- Resident Memory Size (KiB) : The non-swapped physical memory a task has used.

>   **SHR** -- Shared Memory Size (KiB) : The amount of shared memory available to a task, not all of which is typically resident. It simply reflects memory that could be potentially shared with other processes.

>   **S** -- Process Status : The status of the task which can be one of:

> > 'D' = uninterruptible sleep

> > 'R' = running

> > 'S' = sleeping

> > 'T' = traced or stopped

> > 'Z' = zombie

>   **%CPU** -- CPU Usage : The percentage of your CPU that is being used by the process. By default, top displays this as a percentage of a single CPU. On multi-core systems, you can have percentages that are greater than 100%. For example, if 3 cores are at 60% use, top will show a CPU use of 180%. See here for more information. You can toggle this behavior by hitting Shifti while top is running to show the overall percentage of available CPUs in use.

>   **%MEM** -- Memory Usage (RES) : A task's currently used share of available physical memory (RAM).

>   **TIME+** -- CPU Time, hundredths : Total CPU time the task has used since it started.

>   **COMMAND** -- Command Name or Command Line : To see the full command line that launched the process, start top with the ``-c`` flag : ``top -c``.



## Memory Usage
---


### 工具：[free](https://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-sysinfo-memory-usage.html)

The ``free`` command displays the total amount of physical memory and swap space for the system as well as the amount of memory that is used, free, shared, in kernel buffers, and cached.


```shell
             total       used       free     shared    buffers     cached
 Mem:        645712     549720      95992          0     176248     224452
 -/+ buffers/cache:     149020     496692
 Swap:      1310712          0    1310712
```


The command ``free -m`` shows the same information in megabytes, which are easier to read.

```shell
             total       used       free     shared    buffers     cached
Mem:           630        536         93          0        172        219
-/+ buffers/cache:        145        485
Swap:         1279          0       1279
```


### [/proc/meminfo](http://www.binarytides.com/linux-command-check-memory-usage/)

The next way to check memory usage is to read the ``/proc/meminfo`` file. Know that the ``/proc`` file system does not contain real files. They are rather virtual files that contain dynamic information about the kernel and the system.

```shell
$ cat /proc/meminfo
MemTotal:        8167848 kB
MemFree:         1409696 kB
Buffers:          961452 kB
Cached:          2347236 kB
SwapCached:            0 kB
Active:          3124752 kB
Inactive:        2781308 kB
Active(anon):    2603376 kB
Inactive(anon):   309056 kB
Active(file):     521376 kB
Inactive(file):  2472252 kB
Unevictable:        5864 kB
Mlocked:            5880 kB
SwapTotal:       1998844 kB
SwapFree:        1998844 kB
Dirty:              7180 kB
Writeback:             0 kB
AnonPages:       2603272 kB
Mapped:           788380 kB
Shmem:            311596 kB
Slab:             200468 kB
SReclaimable:     151760 kB
SUnreclaim:        48708 kB
KernelStack:        6488 kB
PageTables:        78592 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     6082768 kB
Committed_AS:    9397536 kB
VmallocTotal:   34359738367 kB
VmallocUsed:      420204 kB
VmallocChunk:   34359311104 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:       62464 kB
DirectMap2M:     8316928 kB
```

### [vmstat](http://www.binarytides.com/linux-command-check-memory-usage/)

The ``vmstat`` command with the ``-s`` option, lays out the memory usage statistics much like the ``proc`` command. Here is an example

```shell
$ vmstat -s
      8167848 K total memory
      7449376 K used memory
      3423872 K active memory
      3140312 K inactive memory
       718472 K free memory
      1154464 K buffer memory
      2422876 K swap cache
      1998844 K total swap
            0 K used swap
      1998844 K free swap
       392650 non-nice user cpu ticks
         8073 nice user cpu ticks
        83959 system cpu ticks
     10448341 idle cpu ticks
        91904 IO-wait cpu ticks
            0 IRQ cpu ticks
         2189 softirq cpu ticks
            0 stolen cpu ticks
      2042603 pages paged in
      2614057 pages paged out
            0 pages swapped in
            0 pages swapped out
     42301605 interrupts
     94581566 CPU context switches
   1382755972 boot time
         8567 forks
```


## Disk IO
---

### 工具：[iostat](http://www.tecmint.com/command-line-tools-to-monitor-linux-performance/)

The ``iostat`` is a simple tool that will collects and shows system input and output storage device statistics. This tool is often used to trace storage device performance issues including devices, local disks, remote disks such as NFS.

```shell
Linux 3.10.0-123.20.1.el7.x86_64 (hatch.auto.local) 	05/11/2015 	_x86_64_	(2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.02    0.00    0.01    0.01    0.00   99.95

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
sda               0.12         0.85         0.58     218696     149747
dm-0              0.00         0.00         0.00        904          0
dm-1              0.10         0.70         0.41     180265     106345
dm-2              0.00         0.03         0.16       8475      41342
```


## Network IO
---

### 工具：[ifstat](http://www.binarytides.com/linux-commands-monitor-network/)

The ``ifstat`` reports the network bandwidth in a batch style mode. The output is in a format that is easy to log and parse using other programs or utilities.

```shell
#kernel
Interface        RX Pkts/Rate    TX Pkts/Rate    RX Data/Rate    TX Data/Rate
                 RX Errs/Drop    TX Errs/Drop    RX Over/Rate    TX Coll/Rate
lo                  7756 0          7756 0         1823K 0         1823K 0
                       0 0             0 0             0 0             0 0
enp4s0            226685 0         62294 0       102269K 0         9380K 0
                       0 827           0 0             0 0             0 0
virbr0                 0 0             0 0             0 0             0 0
                       0 0             0 0             0 0             0 0

```


### Read More

+ [Linux Performance Monitoring with Vmstat and Iostat Commands](http://www.tecmint.com/linux-performance-monitoring-with-vmstat-and-iostat-commands/)

+ [18 commands to monitor network bandwidth on Linux server](http://www.binarytides.com/linux-commands-monitor-network/)

+ [Linux性能测试之基准测试工具](http://niyunjiu.iteye.com/blog/316302)

+ [Performance Testing](http://blog.csdn.net/lnxfei/article/details/45866321)

+ [Linux Benchmark Tools](http://www.gnutoolbox.com/linux-benchmark-tools)


##  压力测试工具
---


**CPU** stress


**内存** stress


**磁盘IO** iozone, bonnie++


**网络IO** netperf


##  博客工具
---

|工具|简介|安装配置|星级|详情|备注|
|---|---|---|---|---|---|
|Hexo|一款博客程序|略|★★★★☆|[详情](Tools/Hexo/Hexo.md)|无|
|CSDN博客|GNOME下的下拉式终端|略|★★★☆☆|[详情](Tools/CSDN/CSDN.md)|无|
|Wordpress|免费的开源项目|略|★★★☆☆|[详情](Tools/Wordpress/Wordpress.md)|无|
|Jekyll|简单的免费的Blog生成工具|略|★★★★☆|[详情](Tools/Jekyll/Jekyll.md)|无|
|Octopress|基于Ruby的开源Blogging Framework|略|★★★★☆|[详情](Tools/Octopress/Octopress.md)|无|


###  选择

Wordpress, Jekyll, Octopress用的人都挺多，都挺不错的。我没用过，具体的这里不做评论。介绍下我写博客的方式：Hexo + Github + CSDN博客。
我会在本地搭建Hexo博客框架，然后在该框架中用Markdown的文本格式写博客文档，然后通过Hexo生成静态页面，push到Github上，然后通过Github生成Github博客。最后再将该Markdown文件导入到CSDN博客。所以基本上很容易就实现一式三份：本地一份，CSDN博客一份，Github博客一份。



##  监控应用
---

|工具|简介|安装配置|星级|详情|备注|
|---|---|---|---|---|---|
|Nagios|监视系统运行状态和网络信息的监视系统|略|★★★★☆|[详情](Tools/Nagios/Nagios.md)|无|
|OpenNMS|企业级基于Java/XML的分布式网络和系统监控管理平台|略|★★★☆☆|[详情](Tools/OpenNMS/OpenNMS.md)|无|
|Zabbix|基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案|略|★★★☆☆|[详情](Tools/Zabbix/Zabbix.md)|无|
|Wireshark|网络封包分析软件|略|★★★★☆|[详情](Tools/Wireshark/Wireshark.md)|无|
|Zenoss|开源企业级IT管理软件-是智能监控软件|略|★★★★☆|[详情](Tools/Zenoss/Zenoss.md)|无|
|htop|Linux下的交互式的进程浏览器|略|★★★★☆|[详情](Tools/Htop/Htop.md)|无|
|atop|用来查看Linux系统负载的交互式监控工具|略|★★★★☆|[详情](Tools/Atop/Atop.md)|无|
|top|经典的Linux下的监控命令|略|★★★★☆|[详情](Tools/Top/Top.md)|无|

###  补充

ICINGA项目是 由Michael Luebben、HendrikB?cker和JoergLinge等人发起的，他们都是现有的Nagios项目社区委员会的成员，他们承诺，新的开源项 目将完全兼容以前的Nagios应用程序及扩展功能。在新项目的网站上，他们是如此定义ICINGA的，这将是一个介于Nagios社区版和企业版间的产 品。特别将致力于解决Nagios项目现在的问题，比如不能及时处理Nagios项目的bug、新功能不能及时添加等。还有在新的ICINGA项目中，将 更好的实现数据库集成方面的功能，标准化第三发应用程序的接口等。期待中。

###  选择


监控系统和网络： Nagios &gt; OpenNMS &gt; Zabbix &gt; Wireshark &gt; Zenoss

命令行监控工具: htop，atop，top都不错，可以根据自己的习惯进行选择。


##  Linux终端
---

###  Linux终端快捷键设置

  Setting》》点击Keyboard》》选择左边Shortcuts –>Custom Shortcuts：
点击旁边的+号，然后输入（Name这里随便写）,Command填写/usr/bin/gnome-terminal.
点Apply，然后将它的快捷键设置为Super+Z。

### Linux终端工具

|工具|简介|安装配置|星级|详情|备注|
|---|---|---|---|---|---|
|Yakuake|KDE下的下拉式终端|略|★★★★☆|[详情](Tools/Yakuake/Yakuake.md)|无|
|Guake|GNOME下的下拉式终端|略|★★★☆☆|[详情](Tools/Guake/Guake.md)|无|
|Tilda|下拉式终端|略|★★★☆☆|[详情](Tools/Tilda/Tilda.md)|无|
|Terminator|非下拉式终端|略|★★★★☆|[详情](Tools/Terminator/Terminator.md)|无|
|Stjerm|终端软件|略|★★★★☆|[详情](Tools/Stjerm/Stjerm.md)|无|


----
[↑ 返回上级](https://github.com/qiudeyang/linux-software)

[← 返回首页](https://github.com/qiudeyang/linux-software)
