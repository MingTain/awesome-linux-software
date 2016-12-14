

## 问题汇总

---
### 1. 增加新主机不成功

参照[在已有集群上添加新主机的办法](CDH-install-add-host.md)，在Cloudera Manager中添加新主机时，首先在新主机上启动agent，之后还未在Cloudera Manager界面执行添加新主机操作，却发现该主机已添上并与之前的主机产生混乱，手动添加该主机也不能成功。

之前碰到该问题后，一切重装即可解决，不过解决该问题不必这么粗暴，应该有更好的解决方案。。。

---
### 2. 重装HDFS出错

**错误描述：** 正在检查 NameNode 的名称目录是否为空。仅在为空时格式化 HDFS。

**错误原因：** 旧的hdfs文件会对新安装HDFS产生干扰。

**解决办法：**在namenode和所有datanode上执行`rm -r /dfs`。

---
### 3. 重装oozie报错

**错误描述：**
```
Error: DB schema exists

Stack trace for the error was (for debug purposes):

java.lang.Exception: DB schema exists
	at org.apache.oozie.tools.OozieDBCLI.validateDBSchema(OozieDBCLI.java:929)
	at org.apache.oozie.tools.OozieDBCLI.createDB(OozieDBCLI.java:184)
	at org.apache.oozie.tools.OozieDBCLI.run(OozieDBCLI.java:127)
	at org.apache.oozie.tools.OozieDBCLI.main(OozieDBCLI.java:78)
```

**解决办法：**`rm -rf /var/lib/oozie/*`

---
### 4. 开启oozie Web界面
启动oozie后进入web界面，提示安装ext-2.2后web才能正常显示。参照[官方--Enabling the Oozie Web Console](http://www.cloudera.com/content/www/en-us/documentation/enterprise/latest/topics/admin_oozie_console.html#concept_fl3_35t_2r_unique_1)操作即可解决。

附ext-2.2.zip下载链接－－[ext-2.2.zip](http://tiny.cloudera.com/oozie-ext-2.2)。

---
### 5. 重装Hive出错

**错误1：**连接数据库出错
**解决办法：**见[ 离线安装Cloudera Manager 5和CDH5(5.3.8) --初始化CM5的数据库](http://blog.csdn.net/u012948976/article/details/49702845)。


**错误2：**

	Creating Hive user directory

	Failed to execute command Create Hive User Directory on service Hive

**解决办法：**重启HDFS和yarn后再安装即可。

---
### 6. 作业一直是running

**错误描述：**使用hadoop提交简单的作业--wordcount后，一直处于running状态。在作业界面查看，显示作业状态为“accepted”，“unassigned”。

**问题原因：**内存分配不合理。

**解决办法：**进入YARN的 Configuration，搜索“container”，将“Container Memory”设置为2G，默认为1G，将“Container Memory Maximum”设置为2G，默认为1G。

---
### 7. 退出hadoop安全模式
参考[Hadoop 解除 "Name node is in safe mode" ](http://blog.csdn.net/hongweigg/article/details/7185328)，

	hadoop dfsadmin -safemode leave

---
### 8. 查看hadoop进程
1. 切换到hdfs用户
2. 执行` /usr/java/jdk1.8.0_60/bin/jps`．实际上输入`jps`即可，但前提是要把JAVA_HOME配置正确．

---
### 9. spark-shell启动出错

在命令行执行`spark-shell`之后，报错`NoClassDefFoundError com.apache.hadoop.fs.FSDataInputStream`，

参照[NoClassDefFoundError com.apache.hadoop.fs.FSDataInputStream when execute spark-shell](http://stackoverflow.com/questions/30906412/noclassdeffounderror-com-apache-hadoop-fs-fsdatainputstream-when-execute-spark-s)和[Using Spark's "Hadoop Free" Build](http://spark.apache.org/docs/latest/hadoop-provided.html)解决。

即找到spark-env.sh文件，在其末尾添加`export SPARK_DIST_CLASSPATH=$(hadoop classpath)`即可。注：在CDH中，该文件一般位于`/opt/cloudera/parcels/CDH/etc/spark/conf.dist`目录下。


---
### 10. 修改HDFS的容量

**解决办法：**修改HDFS的配置项--`dfs.datanode.du.reserved`，该参数表示磁盘保留的HDFS不能使用的空间大小。

修改HDFS的配置项--`dfs.data.dir`，为其添加或减少HDFS的本地数据目录（也可以使用命令`hadoop-fuse-dfs`来完成此操作）。


---
### 11. 应对主机ip变化

当主机的IP修改后，集群往往不能识别该主机，请参照[修改Cloudera Manager 管理机器的IP - chenfool - 博客园](http://www.cnblogs.com/chenfool/p/3756066.html)解决。 附[ClouderaManager修改集群的IP ](http://blog.csdn.net/freedomboy319/article/details/52215646)。

---
### 12. 迁移Cloudera Manager
参考[将 Cloudera Manager Server 移至新主机](http://www.cloudera.com/content/www/zh-CN/documentation/enterprise/5-3-x/topics/cm_ag_restore_server.html)和[迁移Cloudera Manager Server 至另一个节点](https://www.zybuluo.com/xtccc/note/186297)。

---
### 13. 集群卸载

+ [官方文档--卸载 Cloudera Manager 和托管软件](http://www.cloudera.com/content/www/zh-CN/documentation/enterprise/5-3-x/topics/cm_ig_uninstall_cm.html)
+ [CDH 5.x 完全卸载指南](http://www.jianshu.com/p/79d1411aaa42)：上述官方文档的中文翻译
+ [干净卸载 Cloudera CDH 5 beta2](http://www.cnblogs.com/chenfool/p/3738540.html)

除按照上述链接进行卸载外，还要删除安装在`/opt`目录下的Cloudera Manager和CDH，以及删除之前创建的cm用户--cloudera-scm（` userdel  cloudera-scm`）.

---
## 更多
以上问题多是在CDH上安装组件（服务）出现的问题，可以参考组件单独安装的完整流程--[CDH 5 组件安装](http://www.cloudera.com/content/www/zh-CN/documentation/enterprise/5-3-x/topics/cdh_ig_cdh5_comp_install.html)来了解每个安装步骤的原理，从而找到问题的根本原因。

----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/Big-Data/Big-Data.md)

[← 返回首页](https://github.com/asin929/linux-software)
