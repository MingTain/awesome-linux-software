

## 安装及配置
### 安装
参照【1】、【2】、【３】，安装Fcix及搜狗输入法，大概流程为:

        1. 卸载系统自带的ibus
        2. 安装fcitx输入法框架
        3. 安装所需的输入法到fcitx框架，比如 fcitx-sunpinyin，sogoupinyin


### Fcitx配置
打开"Fcitx Configuration"，

+ “Input Method”界面配置

![fcitx input methodd configuration](http://img.blog.csdn.net/20160205133656975)

**注意:**根据提示，第一个输入法是不激活的，即任意放一个输入法即可。之后将需要的输入法添加进来即可。

+ Global Config

![fcitx global configuration](http://img.blog.csdn.net/20160205140334411)

图中配置“Lshift”，"SHIFT Both"键为触发fcitx输入法框架，“CTRL_SHIFT”进行输入法切换。

+ Addon->Simplified Chinese To Traditional

![中英文切换](http://img.blog.csdn.net/20160205144349293)

使用Ctrl+Shift+F进行简繁切换


### 具体输入法配置
点击fedora通知栏里的键盘图标，可针对具体的输入法进行不同的配置，包括皮肤，字体等。如下所示，

![fcitx具体输入法配置](http://img.blog.csdn.net/20160205142508487)

比如，设置好搜狗输入法的皮肤和字体后，如下所示

![搜狗输入法](http://img.blog.csdn.net/20160205143255607)



## 问题汇总

---
**问题一**: 开机提示因fcitx启动失败过多而停止运行，如下

![fcitx启动失败](http://img.blog.csdn.net/20160131102523295)

**解决方法**：此问题是因为开机时启动程序过多，导致搜狗输入法无法启动，故只需在命令行输入`fcitx`重启即可。

---
**问题二**: 在TeXstudio中无法输入中文，即无法调用fcitx输入法。

**解决方法**：参考【4】、【5】，添加fcitx对QT软件的支持，命令如下

```
sudo yum install fcitx-qt5
```


## 参考博客
【1】[Fedora 19安装Fcitx输入法并安装搜狗输入法资源包](http://www.hiadmin.org/linux/fedora19-fcitx)

【2】[Fedora Server 21 安装 搜狗拼音输入法](http://www.cnblogs.com/mawanglin2008/p/4320669.html)

【3】[fedora 21 安装搜狗输入法 ](http://blog.csdn.net/wmzy1067111110/article/details/46605121)

【4】[Fcitx官方文档](https://wiki.archlinux.org/index.php/Fcitx_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29#fcitx-sogoupinyin_.E5.8D.A1.E6.AD.BB.E3.80.81.E8.81.94.E6.83.B3.E5.A4.B1.E8.B4.A5)

【5】[Ubuntu下安装Texmaker的问题与解决方案]( http://blog.csdn.net/bendanban/article/details/23336155 )


## 更多

[Linux应用环境实战09：在Linux中使用搜狗拼音输入法以及搞定Flash和支付宝](http://www.cnblogs.com/youxia/p/linux009.html)介绍了在Ubuntu下安装搜狗输入法的方法。


----
[↑ 返回上级](https://github.com/asin929/linux-software/blob/master/Office-Application/Office-Application.md)

[← 返回首页](https://github.com/asin929/linux-software)
