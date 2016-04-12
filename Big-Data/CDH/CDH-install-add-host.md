

说明：本文介绍在已有集群的基础上，添加新主机的方法。之前集群的安装见[CDH安装上篇]()和[CDH安装下篇]()。

---
## 新主机配置

**注意：新主机最好与之前主机的操作系统一致，否则会造成安装不成功。**
+ Hostname: lyhadoop3.com
+ server OS: Ubuntu 14.04 Trusty 64bit
+ server memory:4G
+ server JDK version: jdk8

---
## 安装前准备

[官网－－－Cloudera Manager 5 的要求和支持的版本](http://www.cloudera.com/content/cloudera/zh-CN/documentation/core/v5-3-x/topics/cm_ig_cm_requirements.html)．**注意**:以下操作全部使用root帐户．

### 1. 安装jdk
按照[ linux下jdk安装 ](http://blog.csdn.net/u012948976/article/details/49700227)操作即可. 见[官网－Java Development Kit 安装](http://www.cloudera.com/content/cloudera/zh-CN/documentation/core/v5-3-x/topics/cdh_ig_jdk_installation.html)．

### 2.修改网络配置

#### 修改hostname

        #vi /etc/hostname

#### 修改hosts文件，增加新主机lyhadoop3

        #vi /etc/hosts

        192.168.80.34 lyhadoop.com
        192.168.80.222 lyhadoop2.com
        192.168.80.223 lyhadoop3.com


#### 将新的hosts文件copy到其余所有节点

### 3.关闭防火墙
        #sudo apt-get remove iptables

### 4.SSH无密码登录

#### 安装并启动ssh服务
按照[ ubuntu14下ssh和防火墙设置 ](http://blog.csdn.net/u012948976/article/details/49700703)操作即可．


#### SSH无密码登录－－配置主机无密码登录子节点（主节点）


1. 拷贝认证文件到lyhadoop3

        scp ~/.ssh/authorized_keys root@lyhadoop2.com:~/.ssh/

2. 测试是否可以无密码登录

        ssh lyhadoop3.com

### 5. 安装并配置ntp服务（非必须）
集群中所有主机必须保持时间同步，如果时间相差较大会引起各种问题。 具体思路如下：master节点作为ntp服务器与外界对时中心同步时间，随后对所有datanode节点提供时间同步服务。所有datanode节点以master节点为基础同步时间。

**注意：如果对集群要求不高，比如仅做测试，可以不进行时间同步。若在安装过程中提示主机间时间差过大，请手动修改主机时间。**

#### 安装ntp

        apt-get install ntp


打开ntp配置文件＂/etc/ntp.conf＂，将＂server xxxx prefer＂中的＂xxxx＂替换为所需对时的服务器域名或ip．
执行`service ntpd start`开启ntp服务．执行`ntp stat`查看同步状态．


#### 从节点与主节点对时

在ntp配置文件中增加＂server　master＂；master为主节点的域名或ip，执行`service ntpd start`开启ntp服务．


---
## 正式安装

### 1.下载Cloudera Manager
[Cloudera Manager下载地址](http://archive-primary.cloudera.com/cm5/cm/5/)，按照自身系统下载相应版本即可．我们的系统是ubuntu trusty，故下载＂cloudera-manager-trusty-cm5.3.8_amd64.tar.gz＂．


### 2.解压cloudera-manager

将下载下来的＂cloudera-manager-trusty-cm5.3.8_amd64.tar.gz＂解压，将解压出来的cm-5.3.8放到＂/opt＂目录下．

### 3.创建用户

         useradd --system --home=/opt/cm-5.3.8/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm

### 4.配置 Cloudera Manager Agent

修改/opt/cm-5.3.8/etc/cloudera-scm-agent/config.ini中的server_host为namenode的主机名，server_port不要改动，保持默认值7182即可。**注意：此步骤的目的是为了Server监听到agent，所以一定要做。**

### 5. 启动agent

        /opt/cm-5.3.8/etc/init.d/cloudera-scm-agent start

**注意**：可使用`/opt/cm-5.3.8/etc/init.d/cloudera-scm-server status`和`/opt/cm-5.3.8/etc/init.d/cloudera-scm-agent status`查看启动状态．若启动出错，可查看日志文件夹＂/opt/cm-5.3.8/log＂，里面包含server和agent的log．也可以通过/var/log/cloudera-scm-server/cloudera-scm-server.log，/var/log/cloudera-scm-agent/cloudera-scm-agent.log查看日志．

之后，可以在 Cloudera Manager Admin Console查看到正在运行的该主机，将其拉进相应集群即可。


