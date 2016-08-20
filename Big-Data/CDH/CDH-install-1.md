


# 离线安装离线安装Cloudera Manager 5和CDH5.3.8（上）


## 说明
---
此为离线安装离线安装Cloudera Manager 5和CDH5.3.8，想要了解全部的安装方式，请参阅[CDH安装概览篇](CDH--install-overview.md)。本教程主要参考自官网文档--[Cloudera Enterprise 5.3.x 文档](http://zh-cn.cloudera.com/documentation/enterprise/5-3-x.html)。


---
## 系统环境

+ server OS: Ubuntu 14.04 Trusty 64bit
+ server memory:4G
+ server JDK version: jdk8
+ mysql:

---
## 节点分配

|node|ip| hostname|
|---|---|---|
|namenode  |192.168.80.34  |  lyhadoop.com|
|datanode  |192.168.80.222  |  lyhadoop2.com|


---
## 安装前准备

[官网－－－Cloudera Manager 5 的要求和支持的版本](http://www.cloudera.com/content/cloudera/zh-CN/documentation/core/v5-3-x/topics/cm_ig_cm_requirements.html)．**注意**:以下操作全部使用root帐户．

### 1. 安装jdk（所有节点）
按照[ linux下jdk安装 ](http://blog.csdn.net/u012948976/article/details/49700227)操作即可. 见[官网－Java Development Kit 安装](http://www.cloudera.com/content/cloudera/zh-CN/documentation/core/v5-3-x/topics/cdh_ig_jdk_installation.html)．

### 2.修改网络配置（所有节点）

#### 修改hostname（主节点和从节点分别修改）

        #vi /etc/hostname

#### 修改hosts文件（主节点）

        #vi /etc/hosts

        192.168.80.34 lyhadoop.com
        192.168.80.222 lyhadoop2.com

~~建议将hosts文件里“127.0.0.1 localhost“去掉，避免与上述配置产生冲突。~~

#### 将主节点的hosts文件copy到各从节点

### 3.关闭防火墙（所有节点）
        #sudo apt-get remove iptables

### 4.SSH无密码登录

#### 安装并启动ssh服务（所有节点）
按照[ ubuntu14下ssh和防火墙设置 ](http://blog.csdn.net/u012948976/article/details/49700703)操作即可．

#### SSH无密码登录－－配置主机无密码登录所有子节点（主节点）

1. 在主节点生成公钥文件

        ssh-keygen -t rsa

2. 把公钥添加到认证文件中

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

3. 设置认证文件权限

        chmod 600 ~/.ssh/authorized_keys

4. 拷贝认证文件到所有datanode节点

        scp ~/.ssh/authorized_keys root@lyhadoop2.com:~/.ssh/

5. 测试是否可以无密码登录

        ssh lyhadoop2.com

### 5. 安装并配置ntp服务
集群中所有主机必须保持时间同步，如果时间相差较大会引起各种问题。 具体思路如下：master节点作为ntp服务器与外界对时中心同步时间，随后对所有datanode节点提供时间同步服务。所有datanode节点以master节点为基础同步时间。

#### 安装ntp（所有节点）

        apt-get install ntp

#### 主节点与服务器对时（主节点）

打开ntp配置文件＂/etc/ntp.conf＂，将＂server xxxx prefer＂中的＂xxxx＂替换为所需对时的服务器域名或ip．
执行`service ntpd start`开启ntp服务．执行`ntp stat`查看同步状态．

#### 从节点与主节点对时（从节点）

在ntp配置文件中增加＂server　master＂；master为主节点的域名或ip，执行`service ntpd start`开启ntp服务．

### 6. 安装MySQL（主节点）

按照[官网－－－MySQL 数据库](http://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_ig_mysql.html?scroll=cmig_topic_5_5)一步步操作即可．

安装完MySQL后需要分别创建适用于 Activity Monitor、Reports Manager、Hive Metastore、Sentry Server、Cloudera Navigator Audit Server 和 Cloudera Navigator Metadata Server 的数据库。以Hive为例，

	create database metastore DEFAULT CHARACTER SET utf8;
	grant all on metastore.* TO 'Hive'@'lyhadoop.com' IDENTIFIED BY 'hive_password';

第一句表示创建Hive的数据库metastore ，第二句表示将metastore的所有权限授予主机lyhadoop.com上的用户Hive，Hive的密码为hive_password。在后面安装Hive服务时会用到此处的配置。

**特别注意**：使用MySQL时可能会出现root用户不能正常登录的情况（即使密码正确），此时按照[mysql中root不能登录 ](http://blog.csdn.net/u012948976/article/details/49701311)操作即可．

---
## 正式安装（主节点）
[官网－－－安装路径 C - 使用 Cloudera Manager 原始码手动安装](http://www.cloudera.com/content/cloudera/zh-CN/documentation/core/v5-3-x/topics/cm_ig_install_path_c.html)

### 1.下载并解压Cloudera Manager
[Cloudera Manager下载地址](http://archive-primary.cloudera.com/cm5/cm/5/)，按照自身系统下载相应版本即可．我们的系统是ubuntu trusty，故下载＂cloudera-manager-trusty-cm5.3.8_amd64.tar.gz＂．

将下载下来的＂cloudera-manager-trusty-cm5.3.8_amd64.tar.gz＂解压，将解压出来的cm-5.3.8放到＂/opt＂目录下．

### 2.创建用户（所有节点）

         useradd --system --home=/opt/cm-5.3.8/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm

### 3.配置 Cloudera Manager Agent

修改/opt/cm-5.3.8/etc/cloudera-scm-agent/config.ini中的server_host为namenode的主机名，server_port不要改动，保持默认值7182即可。

### 4.同步Agent到其他节点

        scp -r /opt/cm-5.3.8 root@lyhadoop2.com:/opt/

### 5.初始化CM5的数据库

        /opt/cm-5.3.8/share/cmf/schema/scm_prepare_database.sh mysql cm -h localhost -u root -p --scm-host localhost scm scm scm

上述命令中的“-h localhost”表示安装该数据库的主机的 IP 地址或主机名，“--scm-host localhost”表示安装了 Cloudera Manager Server 的主机名。

scm_prepare_database.sh命令的详情见[官网－－准备 Cloudera Manager Server 外部数据库](http://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_ig_installing_configuring_dbs.html)．

### 6.准备Parcels，用以安装CDH5


[cdh5下载地址](http://archive.cloudera.com/cdh5/parcels/5.3/)，按照自身系统下载如下文件：

+ CDH-5.3.8-1.cdh5.3.8.p0.5-trusty.parcel
+ CDH-5.3.8-1.cdh5.3.8.p0.5-trusty.parcel.sha1
+ manifest.json

**注意**：将＂CDH-5.3.8-1.cdh5.3.8.p0.5-trusty.parcel.sha1＂重命名为＂CDH-5.3.8-1.cdh5.3.8.p0.5-trusty.parcel.sha＂．

将CHD5相关的Parcel包放到主节点的/opt/cloudera/parcel-repo/目录中（parcel-repo需要手动创建）。

相关的文件如下：

+ CDH-5.3.8-1.cdh5.3.8.p0.5-trusty.parcel
+ CDH-5.3.8-1.cdh5.3.8.p0.5-trusty.parcel.sha
+ manifest.json

未完，[见离线安装Cloudera Manager 5和CDH5.3.8（下）](https://github.com/asin929/linux-software/blob/master/Big-Data/CDH/CDH-install-2.md)。

---
## 其它环境安装
参考链接：

[1].[离线安装Cloudera Manager 5和CDH5(最新版5.1.3) 完全教程](http://www.cnblogs.com/jasondan/p/4011153.html)

[2].[离线安装 Cloudera ( CDH 5.x )](http://www.cnblogs.com/modestmt/p/4540818.html)

[3].[CDH离线安装手册](http://blog.selfup.cn/1486.html)

----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/Big-Data/Big-Data.md)

[← 返回首页](https://github.com/asin929/linux-software)
