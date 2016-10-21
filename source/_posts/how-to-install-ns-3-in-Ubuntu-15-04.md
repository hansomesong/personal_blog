title: Ubuntu 15.04下安装NS-3
date: 2015-08-01 20:00:41
keywords: ns-3, ubuntu, 15.04
tags:
---

官方教程

#Prerequists
关于Ubuntu下的Prerequistes，[1]中有很详细的解释。
```shell
#版本管理软件
sudo apt-get install git cvs mercurial
#
sudo apt-get install gcc g++ python python-dev qt4-dev-tools bzr cmake libc6-dev libc6-dev-i386 g++-multilib gdb valgrind gsl-bin libgsl0-dev libgsl0ldbl flex bison libfl-dev sqlite sqlite3 libsqlite3-dev libxml2 libxml2-dev libgtk2.0-0 libgtk2.0-dev vtun lxc uncrustify doxygen graphviz imagemagick python-sphinx dia python-pygraphviz python-kiwi python-pygoocanvas libgoocanvas-dev libboost-signals-dev libboost-filesystem-dev openmpi-bin openmpi-common openmpi-doc libopenmpi-dev p7zip-full rar unrar autoconf 
sudo apt-get install python-pygccxml

#Note: Sphinx version >= 1.12 required for ns-3.15. To check your version, type "sphinx-build". To fetch this package alone, outside of the Ubuntu package system, try "sudo easy_install -U Sphinx".

#跟Tex有关的包，由于体积交大，所以最后再安装
sudo apt-get install texlive texlive-extra-utils texlive-latex-extra
```
#安装
安装方式有好多种，我们选择Bake方法
```shell
mkdir repos && cd repos
hg clone http://code.nsnam.org/bake
export PATH=$PATH:$BAKE_HOME
export PYTHONPATH=$PYTHONPATH:$BAKE_HOME
```
执行./bake.py check, 输出为：
```shell
qsong@sony:~/Documents/repos/bake$ ./bake.py check
 > Python - OK
 > GNU C++ compiler - OK
 > Mercurial - OK
 > CVS - OK
 > GIT - OK
 > Bazaar - OK
 > Tar tool - OK
 > Unzip tool - OK
 > Unrar tool - OK
 > 7z  data compression utility - OK
 > XZ data compression utility - OK
 > Make - OK
 > cMake - OK
 > patch tool - OK
 > autoreconf tool - OK
 > Path searched for tools: /usr/local/sbin /usr/local/bin /usr/sbin /usr/bin /sbin /bin /usr/games /usr/local/games 
 /home/qsong/Documents/repos/bake/bake 
 /home/qsong/Documents/repos/bake/bake bin  
 /home/qsong/Documents/repos/bake/bake
```
上面的输出表明：一切就绪，可以安装NS-3了。首先需要告诉bake我们需要下载代码，build的版本是 3.23.
```shell
./bake.py configure -e ns-3.23
./bake.py show
module: pybindgen-0.17.0.886 (enabled)
  No dependencies!
module: g++ (enabled)
  No dependencies!
module: qt4 (enabled)
  No dependencies!
module: python-dev (enabled)
  No dependencies!
module: pygraphviz (enabled)
  No dependencies!
module: pygoocanvas (enabled)
  No dependencies!
module: netanim-3.106 (enabled)
  depends on:
     qt4 (optional:False)
     g++ (optional:False)
module: pyviz-prerequisites (enabled)
  depends on:
     python-dev (optional:True)
     pygraphviz (optional:True)
     pygoocanvas (optional:True)
module: ns-3.23 (enabled)
  depends on:
     netanim-3.106 (optional:True)
     pybindgen-0.17.0.886 (optional:True)
     pyviz-prerequisites (optional:True)

-- System Dependencies --
 > g++ - OK
 > pygoocanvas - OK
 > pygraphviz - OK
 > python-dev - OK
 > qt4 - OK
```
一切就绪，可以开始Download代码，并且编译(build)
```shell
qsong@sony:~/Documents/repos/bake$ ./bake.py download
 >> Downloading pybindgen-0.17.0.886 - OK
 >> Searching for system dependency g++ - OK
 >> Searching for system dependency qt4 - OK
 >> Searching for system dependency python-dev - OK
 >> Searching for system dependency pygraphviz - OK
 >> Searching for system dependency pygoocanvas - OK
 >> Downloading netanim-3.106 - OK
 >> Downloading ns-3.23 - OK
```
教程里没有提的是，我们需要进入pybindgen-0.17.0.886，执行：
```shell
sudo python setup.py install
```
否则接下来的编译阶段会报错的(其实即使做了上一步还是会有错，出乎意料了，google无果。)
```shell
qsong@sony:~/Documents/repos/bake$ ./bake.py build

```
那不如试试用waf来配置编译吧
```shell
# change into the source directory of ns-3.23
cd source/ns-3.23
# 清楚之前编译配置而生成的文件
./waf clean
# 采用debug的profile来配置
./waf --build-profile=debug --enable-examples --enable-tests configure
# 编译, 不需要打 build 命令
./waf
# 输出结果
Modules built:
antenna                   aodv                      applications
bridge                    buildings                 config-store
core                      csma                      csma-layout
dsdv                      dsr                       energy
fd-net-device             flow-monitor              internet
lr-wpan                   lte                       mesh
mobility                  mpi                       netanim (no Python)
network                   nix-vector-routing        olsr
point-to-point            point-to-point-layout     propagation
sixlowpan                 spectrum                  stats
tap-bridge                test (no Python)          topology-read
uan                       virtual-net-device        visualizer
wave                      wifi                      wimax

Modules not built (see ns-3 tutorial for explanation):
brite                     click                     openflow
```
既然执行test中例子，看是否都可以通过
```shell
qsong@sony:~/Documents/repos/bake/source/ns-3.23$ ./test.py -c core
# 由于输出过程，这里我只贴出部分输出，其他以省略号代替
...
PASS: TestSuite lr-wpan-spectrum-value-helper
PASS: TestSuite lte-cqi-generation
PASS: TestSuite lte-x2-handover-measures
PASS: TestSuite lte-frequency-reuse
...
201 of 204 tests passed (201 passed, 3 skipped, 0 failed, 0 crashed, 0 valgrind errors)
List of SKIPped tests: ns3-tcp-cwnd
    ns3-tcp-interoperability
    nsc-tcp-loss
```
结果满意！

CTTC(加泰罗尼亚理工)对NS-3d的贡献(http://networks.cttc.es/mobile-networks/software-tools/ns-3/)

#参考资料
[1][NS-3官方Wiki安装文档](https://www.nsnam.org/wiki/Installation)
[How do I fix my locale issue?](http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue)
