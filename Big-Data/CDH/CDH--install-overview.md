
## CDH安装概览篇

### 说明
CDH安装有好几种方式，新手容易困惑，故总结之。

### 官网介绍
参见[官网--安装 Cloudera Manager、CDH 和受管服务](http://www.cloudera.com/content/www/zh-CN/documentation/enterprise/5-3-x/topics/cm_ig_intro_to_cm_install.html)，实际上总共有6个阶段，
每个阶段都有不同的方式可以完成。见下图，

![20160803112834801](https://cloud.githubusercontent.com/assets/10795273/19197529/ebcc5536-8cec-11e6-82ae-07b89a3058d3.jpg)


根据上述说明，在安装`Cloudera Manager`阶段时，共有A、B、C三种方式可以完成，
+ 方式A是通过运行`cloudera-manager-installer.bin`文件的方式在线安装
+ 方式B是通过yum在线安装，比如参考文章2
+ 方式C是离线安装，比如[离线安装Cloudera Manager 5和CDH5.3.8（上） ](CDH-install-1.md)


在安装`CDH`阶段时，也有几种方式可以完成，比如
+ 方式1--通过Cloudera Manager安装
+ 方式2--通过Parcel安装，见[离线安装Cloudera Manager 5和CDH5.3.8（上） ](CDH-install-1.md)
+ 方式3--通过yum安装，见[使用yum源安装CDH Hadoop集群 ](http://blog.javachen.com/2013/04/06/install-cloudera-cdh-by-yum.html)。


参考文章1也是不错的理清思路的文章。



## 参考
1. [轻松学Hadoop系列之 - 理清CDH Hadoop集群安装流程 ](http://blog.csdn.net/hezh914/article/details/51190577)
2. [大数据平台CDH集群在线安装](https://segmentfault.com/a/1190000004601093)
