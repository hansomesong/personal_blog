title: Windows 10 和 Ubuntu 15.04 dual-boot 系统安装全攻略
date: 2015-08-01 16:33:47
keywords: windows10, ubuntu, 15.04, dual-boot
tags:
---
2015年7月末，微软终于发布了新一代操作系统Windows 10。据说相对于Windows 8有了非常大的改进，加上利用学校的邮箱可以免费下载到MSNAA的ISO镜像并且附送激活码，更重要的是，现在手里有一台购于2013年西天，型号为SVE141CCW的SONY电脑闲置。所以，我决定抽出一两天的时间，在这台PC上搭建一个 Windows 10&Ubuntu 15.04 双系统：像文字处理，游戏之类的可以在 windows 平台下处理；开发编程Geek相关的东西可以在 Linux(Ubuntu)下来处理。完美！

# 安装 Windows 10
## 下载 Windows 10镜像
国外上学的孩子比较幸福的一点儿就是，可以通过学校的邮箱来免费使用微软收费的软件，比如各种操作系统。现在Windows发布了，我当然不能错过这样的机会了。登陆我的MSNAA账号，发现与以往不同，对于Windows 10 有好多版本，不知所云。
![Available windows 10 version proposed by DreamSpark](http://7xkqa9.com1.z0.glb.clouddn.com/available-win10-version.png)

来看看[Reddit讨论帖][1]里面大家是怎么说吧
>Windows 10 EDU and Windows 10 Multiple versions are probably the only ones of interest to you.
>Windows 10 EDU is a educational version which is similar in feature set to Windows 10 enterprise
>Windows 10 (Multiple Editions) will give you the option to install Windows 10 home or professional.
>The N versions are identical but do not include media software such as windows media player or dvd playback software. These versions are provided as I believe it is a law for the EU to have an option that gives users choice for what software to use rather than the Microsoft default.
>The Features on demand only contains the core basic components of Windows and others can be chosen to be installed.
>The IoT versions are stripped down version meant to be used on low power hardware like a Raspberry Pi.
>The language packs I believe are just provide options for localising Windows in to different languages.
>The debug checked and symbols versions are versions which are more suitable for development and debugging of OS like features. They do not use as much compiler optimisation so identifying and diagnosing operating-system-level problems are easier. So stuff like drivers would be developed on these versions.

关于下载镜像，还有一个插曲：最开始我是通过Mac来下载ISO的，家里网速较慢没有下载完我就去睡觉了，第二天醒来发现下载的ISO文件只有不到3G，当时就很奇怪：不是说 windows 10的安装镜像是3 G多么？为啥我的只有2.3G呢？我想莫非学生优惠版的确实就是小么？后来用这个镜像刻录出来的U盘是无法启动的！再后来我在 Windows 平台又下载同样的文件，显示是3.8G 这才是Win10镜像的正常大小！而且下载速度也是快的多。可惜微软没有提供这些镜像的哈希值，我们也无从校验我们下载的东西是否完整，通过这个事儿，第一次感觉到，下载完大文件之后，核对下哈希值还是很有必要的。


## 制作 Windows 10 USB-bootable 安装盘
现在最流行的操作系统安装方式便是通过制作 USB-bootable 启动盘了。我是用的制作工具是：[Rufus](https://rufus.akeo.ie/)。下面是我当时查看的教程[Create a Windows 10 USB Bootable Flash Drive (Updated)](http://www.groovypost.com/howto/create-windows-10-bootable-usb-flash-drive/)。后来才意思到这其实做的是不支持UEFI的安装盘，其实这也给我带来了些许困扰。因为下面我要说的 Ubuntu 15.04 的安装盘确实支持 UEFI的。虽然我勾选了: MBR partition scheme for BIOS or UEFI computers。写这篇文章的时候才发现还可以通过Rufus来制作 UEFI-supported 的安装盘。请看教程[教你制作支持UEFI PC的Win10安装U盘](http://news.mydrivers.com/1/440/440478.htm)

## UEFI vs Legacy mode
帖子[Can i install windows 10 tech preview along ubuntu 14.04?](http://ubuntuforums.org/showthread.php?t=2248301)中指出，windows installer 会把 Windows boot loader 安装在每一个硬盘分区上。这意味着，如果我们的PC如果之前就安装有 Ubuntu 的话，安装 Windows 之后，我们将没有办法直接登录 Ubuntu 了，需要使用 [Boot Repair](https://help.ubuntu.com/community/Boot-Repair)来做一些修复。

这个UEFI实在是太折磨人了，感觉完全颠覆了我之前的安装系统的习惯啊
用U盘刻录了一个USB-installer，使用Assist->F11(用光盘/USB启动)，结果发现显示opertation system not found。给我郁闷了，好在这是一个常见问题，视频[How To Fix Operating System Not Found Error In Sony Vaio](https://www.youtube.com/watch?v=65Kb3meWeDs)给我解答了我的问题。总结一下思路就是：进入BIOS设置，将Boot mode由 UEFI转换为Legacy, 并且External Bootable device由 disable变为 enable。这个时候就可以使用 win10-usb-installer了。

[UEFI](https://help.ubuntu.com/community/UEFI)
目前主流的启动模式为：UEFI mode 以及 BIOS mode。BISO mode我们都比较熟悉了，这里不做介绍。如今预装win8的机器都采用的UEFI模式。

## 开始安装
因为一个星期前，我才刚把这台可怜的电脑上的 Windows 8 彻底格式化安装了 Ubuntu, 现在我又需要把后者再格式掉，全硬盘安装 Windows 10(可惜了我刚安装好的NS-3)。
```shell
Windows cannot be installed to this disk. The selected disk is of the GPT partition style. Windows cannot be installed to this hard disk space. Windows must be installed to a partition formatted as NTFS. Windows cannot be installed to this hard disk space. The partition is of an unrecognized type.
```
在把所有 Windows installer 无法识别的分区都删除掉之后，我们看到有一个将近700 G的未分配空间，如下图所示。
![全硬盘安装 windows 10](http://7xkqa9.com1.z0.glb.clouddn.com/win10-installation-unallocated-disk.jpg)
很有意思，为了安装 windows 10，系统会额外给我创建一个500 MB的 system reserved 分区处理，特此截图留念：
![为 windows 10 分配磁盘空间](http://7xkqa9.com1.z0.glb.clouddn.com/win10-installation-after-allocation.jpg)


## 硬盘分区
安装完Windows之后，现在就一个硬盘分区，安装完 windows 10，系统文件占了将近20 G，记得以前学校的老师说过，建议把所有的软件都安装到C盘，这样可以避免一些意想不到的问题。所以我觉得分150G 给C盘比较合适，如果以后安装了游戏，可以考虑放到D盘什么。受Linux还有Mac使用经验影响，我决定再只分出一个D盘专门用来存放非系统文件。剩余的将近150G空间则给Linux之用。分区工具我使用的是[分区助手](http://www.disktool.cn/download.html)，确实还是蛮好用的，下图是分区完毕之后：
![](http://7xkqa9.com1.z0.glb.clouddn.com/hard_disk_repartition.PNG)




#安装 Ubuntu 15.04
现在开始安装 Ubuntu 15.04, 第一个麻烦就是：电脑无法识别我U盘里的Ubuntu，又是头一次遇到这种问题，屏幕上只显示：
```shell
SYSLINUX 4.04 EDD 20110518 Copyright (C) 1994-2013 H. Peter Anvin et al
```
还好有人遇到跟我一样的问题，我马上怀疑是不是又是由于 UEFI/legacy mode的缘故。为了安装 Windows 10 我把这个设定由UEFI变成了 Legacy mode，而现在我制作的 Ubuntu安装盘应该是 UEFI模式，只有切换回来，电脑才可以找到操作系统入口，事实证明，我的猜想是正确的。不过在一开始安装Ubuntu的时候，安装程序就检测出来了有非UEFI模式下安装的其他操作系统存在，如下图所示：
![警告](http://7xkqa9.com1.z0.glb.clouddn.com/warning.png)
仔细阅读之后，我觉得我应该选*Go Back*，回过头来看，这里的*Go Back*应该指的是用Legacy mode(而不是UEFI模式)。另外，下图佐证了：我安装的 Windows 10是基于MBR的，因为Disklabel type: dos，如果是基于GPT的，我们应该看到的是：Disklabel type: gpt。
![Ubuntu fdisk 结果](http://7xkqa9.com1.z0.glb.clouddn.com/ubuntu_fdisk_output.png)
![Root分区分配示意图](http://7xkqa9.com1.z0.glb.clouddn.com/partition_root.png)
关于swap分区介绍(https://www.maketecheasier.com/swap-partitions-on-linux/)
设置为primary分区是无法继续分割的。。。
![xxx](http://7xkqa9.com1.z0.glb.clouddn.com/hard_partion_ubuntu_installation.png)
![Swap分区分配示意图](http://7xkqa9.com1.z0.glb.clouddn.com/partition_swap.png)
![最终分区示意图](http://7xkqa9.com1.z0.glb.clouddn.com/partition_resume.png)
其实做完分区开始安装还是蛮紧张的，因为生怕安装了 Ubuntu了以后，就再也没法启动 windows 了，最坏的情况就是，就是两个系统都进不去了(现在想想，好像没有道理无法登陆 Ubuntu)。
![Ubuntu update-grub 结果](http://7xkqa9.com1.z0.glb.clouddn.com/update_grub_output.png)
因为是在UEFI模式下读取的U盘但是安装的是非UEFI模式，所以开机的时候，又是Operation system not found。选择回 Legacy mode，终于顺利启动！
为了进一步验证，现在Ubuntu处于Legacy模式下，输入下面命令：
```shell
#如果/sys/firmware/efi文件夹存在，则显示"EFI boot on HDD"。不然则显示"Legacy boot on HDD"
[ -d /sys/firmware/efi ] && echo "EFI boot on HDD" || echo "Legacy boot on HDD"
```
结果显示为"Legacy boot on HDD"。(曾经显示为EFI boot on HDD)

最后还有个问题就是：每次在Windows和Ubuntu之间切换时间就不一致了。


[1]: http://www.reddit.com/r/Windows10/comments/3f14bg/win10_just_popped_up_in_my_dreamspark_premium_but/, "reddit"
# 参考资料
[Installing Ubuntu on a Pre-Installed Windows 8 (64-bit) System (UEFI Supported)](http://askubuntu.com/questions/221835/installing-ubuntu-on-a-pre-installed-windows-8-64-bit-system-uefi-supported)
[Tip for dual-booting Windows 10 preview and Linux on a PC with UEFI firmware](http://linuxbsdos.com/2015/01/18/tip-for-dual-booting-windows-10-preview-and-linux-on-a-pc-with-uefi-firmware/)
