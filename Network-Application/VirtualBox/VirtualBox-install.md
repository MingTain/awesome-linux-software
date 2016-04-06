
装了两天才装好，赶紧记录一下。

---
## 安装
1. 采用命令行方式安装
`yum install VirtualBox`
安装时，系统会解析依赖，自动选择合适的VirtualBox及kernel版本。比如安装后，我的VirtualBox版本为4.3.32，kerne版本为4.1.13。
可通过命令`uname -r`查看kernel版本号，VirtualBox的版本则直接在软件中查看。

2. 升级系统软件及重启
`sudo yum -y update`
`reboot`
由于是升级了内核，故需进行上述操作。

---
## 配置

安装Guest Additions（客户端增强包)和Extension Pack（扩展增强包），两包的详细含义及区别见[VirtualBox中的Guest Additions（客户端增强包)和Extension Pack（扩展增强包）的含义和区别](http://www.crifan.com/virtualbox_guest_addtions_vs_extension_pack/)。关于Guest Additions的更多详情，请见官网－－[Chapter 4. Guest Additions](http://www.virtualbox.org/manual/ch04.html#idp46457703730512)。

Guest Additions的主要作用是为了共享剪贴板，共享文件夹，无缝窗口，3D虚拟化显示等，在所安装的虚拟机（客户端）进行。**注意：即使在设置里选择了使用双向剪贴板，往往仍然不能在主机和虚拟机里实现剪贴板的共享，此时必须得装Guest Additions。**

+ **Guest Additions的安装（客户端进行）：**
从[virtualbox/4.3.32](http://download.virtualbox.org/virtualbox/4.3.32/)处下载VBoxGuestAdditions_4.3.32.iso，并加载到虚拟机里。之后运行虚拟机，点击“Devices”->“Insert Guest Additions CD Image”，安装Guest Additions包，安装完成后会自动进入虚拟光盘的文件夹，该文件夹提供多种操作系统对应的运行命令。由于我安装的虚拟机为linux，故执行如下命令

        cd /media/asin/VBOXADDITIONS_4.3.32_103443/
        sudo sh ./VBoxLinuxAdditions.run
若找不到目录`/media/asin/VBOXADDITIONS_4.3.32_103443`，则需在文件夹界面点击一下该文件，使其挂载进来。之后，重启虚拟机，即可共享剪贴板。
参考[virtualbox中ubuntu和windows共享文件夹设置](http://www.cnblogs.com/linjiqin/p/3615477.html)和[virtualbox文件夹共享，ubuntu无法自动挂载解决方法](http://blog.sina.com.cn/s/blog_71643ce10101hh2e.html)实现文件夹共享，值得注意是选择共享文件时不要勾选“自动挂载”。另外，实现开机自动挂载的方法主要有两种：一种是修改/etc/fstab文件，不过我按此方法设置，重新开启虚拟机时进不去了。另外一种方式是将挂载命令写到/etc/rc.local文件，该方法能正常执行。

+ **Extension Pack包的安装（主机进行）：**
去官网[virtualbox/4.3.32](http://download.virtualbox.org/virtualbox/4.3.32/)下载"Oracle_VM_VirtualBox_Extension_Pack-4.3.32.vbox-extpack"后，右击使用VirtualBox打开即可完成安装。

---
## 问题解决

### 问题１
启动VirtualBox后，出现如下问题：
![virtualbox问题](http://img.blog.csdn.net/20160205103803138)

首先，按提示，执行`sudo yum install kmod-VirtualBox-$(uname -r) kmod-VirtualBox`安装所需软件。安装后，仍会出现如上问题。

在命令后输入`virtualbox`启动VirtualBox，报错如下
```
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
         available for the current kernel (3.11.4-201.fc19.x86_64) or it failed to
         load. Please make sure that you have kmod-VirtualBox for current kernel and load the kernel module by executing

           'systemctl restart systemd-modules-load.service' (as root)

         You will not be able to start VMs until this problem is fixed.
```
参照[Virtual Box on Fedora 19 fails to start a VM](https://ask.fedoraproject.org/en/question/34470/virtual-box-on-fedora-19-fails-to-start-a-vm/?answer=59222#post-id-59222)，[VirtualBox not working on Fedora 20](https://ask.fedoraproject.org/en/question/54155/virtualbox-not-working-on-fedora-20/)，[VBox & VMware in Secure Boot Linux](http://gorka.eguileor.com/category/technology/linux/)解决该问题。

### 问题２
运行虚拟机时出错，`Failed to open a session for the virtual machine Mac. VT-x is disabled`，即需要开启开启笔记本的Virtualization Technology虚拟化技术功能。不同电脑的开启方法不同，我的是ThinkPad，开机时按F1键进入BIOS，在“Security”里面选择"virtualization"为 enable。

---
## 安装deepin15.1和 Mac OS X Mavericks

### 安装deepin15.1
最近很火的操作系统－－[深度操作系统](http://www.deepin.org//)。界面很美观，融合了Linux，Mac，Windows的优点，使用linux的用户再也不用折腾了。
直接在官网下载镜像，在VirtualBox中添加该虚拟机即可。
![virtualbox-deepin15](http://img.blog.csdn.net/20160205110723931)


### 安装Mac OS X Mavericks
安装Mac OS X Mavericks系统。完全参照[ Ubuntu下使用VirtualBox安装Mac OS X Mavericks(10.9)上篇 ](http://blog.csdn.net/iAm333/article/details/38556757)和[ Ubuntu下使用VirtualBox安装Mac OS X Mavericks(10.9)下篇 ](http://blog.csdn.net/iAm333/article/details/38557381#comments)安装即可。
![virtualbox-mac](http://img.blog.csdn.net/20160213162543537)

----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/Network-Application/Network-Application.md)

[← 返回首页](https://github.com/asin929/linux-software)
