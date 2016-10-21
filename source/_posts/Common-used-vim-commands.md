title: Common-used-vim-commands
date: 2015-07-30 23:59:36
keywords: VIM
tags:
---

利用此贴记录下自己常用的VIM的命令，备忘：
在command mode下，键入
shift+G:    跳转至文章的最后一行
gg:         跳转至文章的开头
o:          跳转至光标当前所在行的下一行并进入insert mode


Error trying to parse settings: Unexpected character, expected a comma or closing bracket in Packages/OmniMarkupPreviewer/OmniMarkupPreviewer.sublime-settings:88:103

[35 Practical Examples of Linux Find Command](http://www.tecmint.com/35-practical-examples-of-linux-find-command/)



网页链接[Mac Office 2016正式版下载附破解补丁](http://www.ruanman.net/archives/7623.html#comments)里面较全面详细地介绍了如何下载，安装以及破解Office 2016 for Mac.



问题在于：



需要一个Microsoft account这个还蛮有意思的，这是在效仿Apple嘛。在开机初始化阶段关联一个Account。

安装完之后，无线网卡的驱动是有的，这点儿还是蛮欣慰的。至少这样我有网络连接去下载其他驱动啊。显卡驱动显然没有安装，画面看着真是巨渣。

其实升级到Win10后无需再像以前那样繁琐地检测系统硬件、显卡型号，然后再到处搜索下载驱动程序。在Windows 10中，只需通过Windows更新即可方便地下载安装驱动程序。开始->settting->update&security->Windows update



#汉字字符显示问题
关键：将non-Unicode编码为 汉语

#应用程序安装 
第一个先安装Google chrome
改变默认程序的方法：
To chagne your default apps, go to Settings->System->Default apps
第二个是office
QQ, skype之类的还有
其他也确实想不到该去安装什么了(目前)

Ubuntu的社区对Grub2有很好的说明的。

#Ubuntu开发环境搭建
(这个其实可以专门写个文章了)
更新系统软件仓库
```shell
sudo apt-get udpate
```
安装openssh-server, 以便从mac上ssh进入这台机器, 安装VIM，以方便编辑配置文件
```shell
sudo apt-get install openssh-server vim
ps aux | grep sshd
```
如果发现有sshd守护进程运行，则说明openssh server成功安装并且启动。
接下来在Ubuntu下创建公钥私钥，以便可以不需要密码接入
```shell
ssh-keygen -t rsa
```
接下来将Mac上的公钥copy到Ubuntu下
```shell
#ssh-copy-id可以通过Homebrew安装
ssh-copy-id qsong@10.35.130.207
```
还是不习惯使用Utity界面，还是使用传统界面吧
```shell
sudo apt-get install gnome-session-flashback
```
安装完毕之后，log out再log in就生效了。
用命令行访问 Windows 10 分区
```shell
#首先需要知道 哪个分区(sdaX)是我们需要访问的分区
cat /proc/partitions
#接下来在/mnt创建一个文件夹，用以挂载我们需要访问的分区
mkdir /mnt/win
#接下来, 挂载分区。注意将X换成相应的数字，比如3
mount /dev/sdaX /mnt/win
#我们可以通过命令行访问分区了
cd /mnt/win
```
不过，现在 Ubuntu 系统都会默认把 windows partition 挂载在某个挂载点的(通常在/media之下)。如何获取哪个分区挂载哪个挂载点之下呢？有以下几种方法
```shell
#方法一：
cat /etc/mtab
#方法二：
df
```




