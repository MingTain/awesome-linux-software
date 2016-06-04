


---
## 安装
+ fedora下执行：`sudo yum install mysql-server`。
**注意：fedora21中mysql正被MariaDB替代．**

+ ubuntu下执行：`sudo apt-get install mysql-server`

---
## 启动
+ fedora下执行：`sudo service mysqld start`。
**注意：fedora21中上述命令已被替换为`/bin/systemctl start mysqld.service`，考虑到mysql正被mariadb替代，故最新的命令为`/bin/systemctl start mariadb.service`．**

+ ubuntu下执行：`sudo service mysql start`

相应的，停止及查看状态只需将＂start＂替换成＂stop＂，＂status＂即可．

---
## 设置开机启动
+ fedora下执行：
```
$ sudo /sbin/chkconfig mysqld on
$ sudo /sbin/chkconfig --list mysqld
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
```

+ ubuntu下执行：` sudo chkconfig mysql on`。
**注意：chkconfig在最新版的ubuntu中已被替换成了[Upstart](http://upstart.ubuntu.com/cookbook/#debian-specific-and-ubuntu-specific-content-debian-and-ubuntu-specific)**．

---
## 设置 MySQL 根密码

+ 方式1
```
mysqladmin -u root password 'your_password'
```

+ 方式2
```
$ sudo /usr/bin/mysql_secure_installation
[...]
Enter current password for root (enter for none):
OK, successfully used password, moving on...
[...]
Set root password? [Y/n] y
New password:
Re-enter new password:
Remove anonymous users? [Y/n] Y
[...]
Disallow root login remotely? [Y/n] N
[...]
Remove test database and access to it [Y/n] Y
[...]
Reload privilege tables now? [Y/n] Y
All done!
```

---
## 问题汇总

### 问题１－－－mysql中root不能登录
环境：ubuntu14

新版的mysql已经要求在安装前就设置root密码，故此问题已经不会再出现。
===================更新===================

问题：使用MySQL时可能会出现root用户不能正常登录的情况（即使密码正确），此时可按照下列步骤重置root密码．

1. 停止mysql

        service mysql stop
2. 安全模式下启动mysql

        mysqld_safe --skip-grant-tables &
3. 以root用户登录mysql

        mysql -u root
4. 更新root密码

        mysql> use mysql;
        mysql> update user set password=password('654321') where user='root' and host='localhost';
        mysql> flush privileges;
        mysql> quit;
5. 退出安全模式

        kill -9 mysqld_safe_pid
6. 开启mysql

        service mysql start

7. **root身份登录**
这一步非常坑，只能使用如下命令登录，其它任何命令包括`mysql`，`mysql -u root`均不行。

        mysql -u root -p



----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/Database/Database.md)

[← 返回首页](https://github.com/asin929/linux-software)
