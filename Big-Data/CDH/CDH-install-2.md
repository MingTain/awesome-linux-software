续上篇[离线安装Cloudera Manager 5和CDH5.3.8（上） ](https://github.com/asin929/linux-software/blob/master/Big-Data/CDH/CDH-install-1.md)。

---
## 部署
### 1.启动server（主节点）

        /opt/cm-5.3.8/etc/init.d/cloudera-scm-server start

### 2.启动agent（所有节点）

        /opt/cm-5.3.8/etc/init.d/cloudera-scm-agent start

**注意**：可使用`/opt/cm-5.3.8/etc/init.d/cloudera-scm-server status`和`/opt/cm-5.3.8/etc/init.d/cloudera-scm-agent status`查看启动状态．若启动出错，可查看日志文件夹＂/opt/cm-5.3.8/log＂，里面包含server和agent的log．也可以通过/var/log/cloudera-scm-server/cloudera-scm-server.log，/var/log/cloudera-scm-agent/cloudera-scm-agent.log查看日志．

### 3.登录 Cloudera Manager Admin Console

登录＂http://localhost:7180/＂（由于CM Server的启动需要花点时间，这里可能要等待一会才能访问），默认的用户名和密码均为admin．

### 4.安装配置注意1

一路点击默认即可．中间过程可能会出现警告：

	Cloudera 建议将 /proc/sys/vm/swappiness 设置为 0。当前设置为 60。使用 sysctl 命令在运行时更改该设置并编辑 /etc/sysctl.conf 以在重启后保存该设置。您可以继续进行安装，但可能会遇到问题，Cloudera Manager 报告您的主机由于交换运行状况不佳。以下主机受到影响：
在终端输入`echo 0 > /proc/sys/vm/swappiness`即可。

若出现其它错误，请查看安装日志．

### 5.安装配置注意2

安装某些服务时，需要进行数据库配置。比如安装Hive时，会出现如下界面

![cdh-hive-database](https://cloud.githubusercontent.com/assets/10795273/14522772/c0637916-0262-11e6-920a-b0d895577453.png)

上述框中需要填写”数据库主机名称“，”数据库类型“，‘’数据库名称”，用户名及密码，可以参考界面下面的帮助。据此类推，实际上可以在Mysql中执行如下命令，表示root用户具有所有数据库的权限，这样添加所有服务时，只需统一填写root用户就可以了。

	grant all on *.* TO 'root'@'lyhadoop.com' IDENTIFIED BY 'root_password';

![hive--数据库配置1](http://img.blog.csdn.net/20151219005105186)
![hive--数据库配置2](http://img.blog.csdn.net/20151219004458795)




---
## 安装错误汇总

### 1.error1

**问题描述：**启动agent时失败，检查日志文件，发现如下

    /opt/cloudera-manager/cm-5.3.8/lib64/cmf/agent/build/env/bin/python: error while loading shared libraries: libpython2.4.so.1.0: cannot open shared object file: No such file or directory

**解决办法：**出现该问题是因为下错了cloudera-manager的版本，下载成了＂cloudera-manager-el5-cm5.3.8_x86_64.tar.gz＂，而这是Red Hat系统的．

### 2.error2

**问题更新:**发现此问题在namenode上比较常见，而在datanode上很少出现。所以可以确定是因为在namenode上往往首先启动cloudera-scm-server，就紧接着启动cloudera-scm-agent，而cloudera-scm-server在启动期间需占用大量资源，导致cloudera-scm-agent启动失败。所以可以在cloudera-scm-server启动一段时间后（大概十分钟）再启动cloudera-scm-agent。

至于datanode上也启动失败，说明当前系统资源不足，kill掉并稍等片刻重启即可。

==========================================更新线===========================================

**问题描述：**启动agent时成功，但随后检查启动状态，会报如下错误，意味着agent启动后随即失败．

    Checking for service cloudera-scm-agent * cloudera-scm-agent is dead and pid file exists

查看cloudera-scm-agent.log，报错：

    Failed to connect to newly launched supervisor. Agent will exit`

查看supervisord.out，报错

    Error: Another program is already listening on a port that one of our HTTP servers is configured to use.  Shut this program down first before starting supervisord.

**解决办法：**参考stackoverflow相关问题－－－[Stopping supervisord: Shut down](http://stackoverflow.com/questions/14479894/stopping-supervisord-shut-down)，执行`ps -ef | grep supervisord`，找到与angent相关的线程并kill掉即可，之后重启agent，并执行`/opt/cm-5.3.8/etc/init.d/cloudera-scm-agent status`查看启动状态。**注意：有时需要重上述操作两三次才可解决该问题。**


### 3.error3

**问题描述：**

    Java is not installed on the hosts of this cluster.

**解决办法：**之前的jdk在＂/usr/local＂目录下，将其移动到＂/usr/java＂下即可．

---
## 其它环境安装

[1].[离线安装Cloudera Manager 5和CDH5(最新版5.1.3) 完全教程](http://www.cnblogs.com/jasondan/p/4011153.html)

[2].[离线安装 Cloudera ( CDH 5.x )](http://www.cnblogs.com/modestmt/p/4540818.html)

[3].[CDH离线安装手册](http://blog.selfup.cn/1486.html)



----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/Big-Data/Big-Data.md)

[← 返回首页](https://github.com/asin929/linux-software)
