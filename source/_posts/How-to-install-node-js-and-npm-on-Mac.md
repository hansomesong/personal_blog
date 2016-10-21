title: 如何在Mac Yosemite中安装node.js以及NPM
date: 2015-07-29 21:04:34
keywords: Node.js,npm,mac,Yosemite
tags:
---

想要使用Githubs pages和hexo搭建一个静态博客站点，因为hexo是台湾小伙儿tommy3551使用Node.js开发的一个模板引擎，以及我的工作电脑的操作系统系统是Mac 10.10.4 (Yosemite)因此，为了使用hexo, 我首先需要在Mac下安装node以及npm。经过调查，发现Node js在Mac下有两种安装方式:
* 基于pkg installer安装
* 通过Homebrew安装
* 通过NVM安装
由于最近使用Homebrew比较多，以及Homebrew以其卓越的用户体验，让我渐渐觉得，好像Mac下凡是跟开发有关的工具/环境都应该通过brew安装。登陆Node.js的官网，看到还有pkg安装包的安装包的方式，猛一下的困惑了，这两种方式我究竟该用哪个呢？

#误入“歧途”
Google is your teacher. 习惯性的先上网搜了一下，Stack overflow上有一篇帖子讨论了在Mac下究竟哪种方式更方便。看完之后，我感觉，哦，好像直接使用pkg安装包更合适了，心想也是啊，其他开发工具，没有pkg我才使用Homebrew呢，现在有pkg installer,我干嘛不用呢？

于是，直接下载安装一气呵成。使用的时候，比如输入
```shell
npm install hexo -g
```
系统提示需要管理员权限，Mac下不用sudo好多年啊，好吧，那就sudo一下呗，心想，要是用brew安装不久没这个不便了么，其实我应该使用brew的呀。再后来，使用bew的时候，习惯性地来了个
```shell
$brew doctor
```
输出显现，/usr/local下面有跟node有关的文件，不是被brew管理的，虽然我不是个有洁癖或者强迫症的人，但是看了一下基本上/usr/local/bin里面的文件，基本owner都是自己啊，现在突然多了一下root的感觉还是怪不爽的，年轻人就是喜欢折腾，我还是删了pkg安装的node改用brew再安装一遍吧。自己就是作啊。。。

#弃暗投明
第一步需要删除已安装的node, 这个帖子：https://gist.github.com/TonyMtz/d75101d9bdf764c890ef 好评如潮，事实证明，确实是好用，不过我对其略有补充，所以这里再把使用的命令粘过来。
```shell
# First list what files are installed by pkg-installer with command lsbom
# Use a loop in shell to delete them
lsbom -f -l -s -pf /var/db/receipts/org.nodejs.pkg.bom | while read f; do  sudo rm /usr/local/${f}; done
# Then delete all node-related bin files under /usr/local/bin
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*

# To recap, the best way (I've found) to completely uninstall node + npm is to do the following:
#go to /usr/local/lib and delete any node and node_modules
cd /usr/local/lib
sudo rm -rf node*

# go to /usr/local/include and delete any node and node_modules directory
cd /usr/local/include
sudo rm -rf node*
```
现在我们可以使用Homebrew来安装node.js以及npm了，很简单：
```shell
brew instal node
```
应该会成功。。。吧。。。
#柳暗花明
不过，让我震惊的是，我居然得到了一些错误信息：
```shell
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
Error: The `brew link` step did not complete successfully
The formula built, but is not symlinked into /usr/local
Could not symlink share/systemtap/tapset/node.stp
/usr/local/share/systemtap/tapset is not writable.
```
我第一反应就是：还是之前由于pkg安装后的遗毒啊。网上有些人遇到了跟我一样的问题，比如这篇帖子：[Trouble install node.js with homebrew](http://stackoverflow.com/questions/31374143/trouble-install-node-js-with-homebrew)。这篇帖子印证了我的猜想，我猜测这是因为pkg包安装node的时候，以root身份创建了文件/usr/local/share/systemtap/tapset。现在Homebrew再来安装的时候，没有办法对该文件读写。于是爆出了上述错误。好的补救措施如下：
```shell
# 将yourusername以及yourgroupename换成你需要的名字
chown -R <yourusername>:<yourgroupname> systemtap
brew link node
```

现在大功告成啦！

#风云再起
2016年伊始，又想重新开始使用hexo记录博客了，没想到hexo不工作了，而且越倒腾越乱，好像网友有人推荐彻底卸载node npm hexo重新再安装，于是乎，发现了一种更灵活的node体系安装方法：通过NVM（Node Version Manager）,
以下是网友给出的很详细的教程：(http://icarus4.logdown.com/posts/175092-nodejs-installation-guide)

#总结
其实并不是用Homebrew安装node+npm就一定比用编译好的pkg安装包好，只是我个人的喜欢觉得还是用brew管理的更好而已，重要的是选择自己用着觉着顺手的就行。

#参考资料
[How to Install Node.js and NPM on a Mac](http://blog.teamtreehouse.com/install-node-js-npm-mac)








