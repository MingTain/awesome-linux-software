
---
## 目录

[TOC]


---
## 安装说明

安装参考官方文档：[Cloudera Enterprise 5.8.x Documentation](http://www.cloudera.com/documentation/enterprise/latest.html)和[Installation Path C - Manual Installation Using Cloudera Manager Tarballs](http://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_install_path_c.html)进行即可。

不过考虑到跟Cloudera Manager5.3.8安装几乎相同，所以可以完全参照[离线安装离线安装Cloudera Manager 5和CDH5.3.8（上）](https://github.com/asin929/linux-software/blob/master/Big-Data/CDH/CDH-install-1.md)和[离线安装离线安装Cloudera Manager 5和CDH5.3.8（下）](https://github.com/asin929/linux-software/blob/master/Big-Data/CDH/CDH-install-2.md)进行。

不过，Cloudera Manager5.8.0的安装终究还是与之前有些不同，记录如下。

---
## 有关数据库配置

CM5.8.0中，新增了对Oozie和Hue数据库的配置。


### 关于Oozie数据库配置
见[Configuring an External Database for Oozie](http://www.cloudera.com/documentation/enterprise/latest/topics/install_oozie_ext_db.html#admin_oozie_ext_db)。

根据上述说明，为Oozie配置数据源时，还要增加MySQL JDBC Driver JAR，
```
Add the MySQL JDBC Driver JAR to Oozie
Copy or symbolically link the MySQL JDBC driver JAR into one of the following directories:

    For installations that use packages: /var/lib/oozie/
    For installations that use parcels: /opt/cloudera/parcels/CDH/lib/oozie/lib/

directory. 
```

因为系统为Ubuntu，我们通过命令` sudo apt-get install libmysql-java`安装`MySQL JDBC Driver`（参考[MySQL Database](http://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_mysql.html#cmig_topic_5_5_2)），安装之后的驱动位于` /usr/share/java/mysql-connector-java.jar`。将该包复制到上述两个目录即可（我没有复制，倒也没有出现错误）。


### 关于Hue数据库配置

见[Using an External Database for Hue Using Cloudera Manager](http://www.cloudera.com/documentation/enterprise/latest/topics/cm_mc_hue_service.html)或[Using an External Database for Hue Using the Command Line](http://www.cloudera.com/documentation/enterprise/latest/topics/cdh_ig_hue_database.html#concept_id1_wkj_zj)。


### 附--其他数据库的配置

打开CM Server的web后，出现
```
请求 Service Monitor 超时。这可能会导致页面响应缓慢。请查看 Service Monitor 的状态。

请求 Host Monitor 超时。这可能会导致页面响应缓慢。请查看 Host Monitor 的状态。
```

原来是没有给Service Monitor和Host Monitor创建数据库，默认的数据库表可以参照[Creating Databases for Activity Monitor, Reports Manager, Hive Metastore Server, Sentry Server, Cloudera Navigator Audit Server, and Cloudera Navigator Metadata Server](http://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_mysql.html#concept_dsg_3mq_bl)进行配置，当然也可以在CM Server的数据库配置界面--`http://cm-server-host:7180/cmf/config2?task=ALL_DATABASES`修改默认配置。


---
## 错误解决


### 错误1--CM Server启动出错

启动CM Server（`/opt/cm-5.8.0/etc/init.d/cloudera-scm-server start`）时报错，

```
/opt/cm-5.8.0/etc/init.d/cloudera-scm-server: line 292: [[: [error] JVM MaxHeap is set less than 2G, CM server may run out of Heap space. Update CMF_JAVA_OPTS in /opt/cm-5.8.0/etc/default/cloudera-scm-server to fix this.: syntax error: operand expected (error token is "[error] JVM MaxHeap is set less than 2G, CM server may run out of Heap space. Update CMF_JAVA_OPTS in /opt/cm-5.8.0/etc/default/cloudera-scm-server to fix this.")
```

看来得调整`CMF_JAVA_OPTS`来设置Java堆的大小，不过暂时没有调整，目前也没有影响。


### 错误2--CM Agent启动出错

启动CM Agent（`/opt/cm-5.8.0/etc/init.d/cloudera-scm-agent start`）时报错，不能启动。查看启动日志，

```
Unable to create the pidfile.
```

解决办法:
```
mkdir /opt/cm-5.8.0/run/cloudera-scm-agent
chown -R cloudera-scm:cloudera-scm /opt/cm-5.8.0/run/cloudera-scm-agent
```

### 错误3--spark启动出错

命令行执行`spark-shell`，报错，


```
java.lang.IllegalArgumentException: Required executor memory (1024+384 MB) is above the max threshold (1024 MB) of this cluster! Please check the values of 'yarn.scheduler.maximum-allocation-mb' and/or 'yarn.nodemanager.resource.memory-mb'
```
在Yarn中同时修改上述两个参数的值，调整到2G。


---
## 附录
关于CM5.8.0中各服务的版本号，
![cdh-5.8-component-version-1](http://img.blog.csdn.net/20160822090915187)
![cdh-5.8-component-version-2](http://img.blog.csdn.net/20160822091008996)


---
## 参考
[1][ cloudera-manager-el6-cm5.7.0版本启动Agent失败的问题解决 ](http://blog.csdn.net/jiangshouzhuang/article/details/52020518)
[2][ Cloudera Manager 5和CDH5离线安装,CDH-5.7.1 ](http://blog.csdn.net/a921122/article/details/51939692)
