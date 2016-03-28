
---
##  1. gnome-tweak-tool
通过命令`sudo yum install gnome-tweak-tool`安装

---
### 1.1 基本设置
该工具可以对系统进行很多个性化的设置，包括字体，键盘，桌面，电源等．

---
### 1.2 安装GNOME Shell扩展
在gnome-tweak-tool的＂Extensions＂中安装扩展．

在[扩展官网](https://extensions.gnome.org/)有很多好用的扩展，比如：

+ [TaskBar](https://extensions.gnome.org/extension/584/taskbar/)：可以在桌面显示应用图标

    ![TaskBar](http://img.blog.csdn.net/20151013164002849)

+ [NetSpeed](https://extensions.gnome.org/extension/104/netspeed/)：可以显示网速

	![NetSpeed](http://img.blog.csdn.net/20151013164136920)

还有好多比较好玩的扩展，大家可以到扩展中心下载体验一下．

---
### 1.3 安装主题
在gnome-tweak-tool的＂Appearance＂中安装主题．

在[GNOME-LOOK.ORG](http://gnome-look.org/?xsection=home)可以下载各类主题．

主题是分类下载的，比如下载GTK主题[Zukimac](http://gnome-look.org/content/show.php/Zukimac?content=165450)，下载后将其解压，在解压的文件中挑选合适的版本放在`/usr/share/themes`目录下，即可设置GTK+主题为Zukimac．

同样的，下载图标主题[Emerald](http://gnome-look.org/content/show.php/Emerald?content=167606)，下载后解压挑选合适版本放到`/usr/share/icons`下，即可设置Icons主题为Emerald．

值的注意的是，上述下载的主题均放到了`/usr/share`目录下，表示任何用户都可以使用．若只想自己使用，则在自己的根目录下建立相应文件＂.themes＂和＂.icons＂即可．

上述效果见：

![themes](http://img.blog.csdn.net/20151013172206432)

---
## 2. Docky

Docky是在桌面底部显示应用图标的，能达到跟Mac一样的效果，Fedora下直接安装即可：`sudo yum install docky`．虽然teak-tool中的TaskBar能实现类似效果，但还是差一点．效果图见：
![Docky](http://img.blog.csdn.net/20151013171844228)

---
## 参考资料

1. [Fedora安装](http://www.cnblogs.com/pengdonglin137/p/3531797.html)
2. [安装Fedora 21工作站后要做的10件事情](http://www.csdn.net/article/2015-01-07/2823459)
3. [ fedora 16 gnome更换主题 ](http://blog.csdn.net/gaoxin1076/article/details/7058625)
4. [Linux应用环境实战12：桌面美化那点事儿](http://www.cnblogs.com/youxia/p/linux012.html)

----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/System-Beautification/System-Beautification.md)

[← 返回首页](https://github.com/asin929/linux-software)
