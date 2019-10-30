# centos-install-python3
#centos下安装python3步骤

1.首先查看系统中python的位置在哪：

[root@root ~]# whereis python

 python: /usr/bin/python2.7 /usr/bin/python /usr/lib/python2.7 /usr/lib64/python2.7 /etc/python /usr/include/python2.7    /usr/share/man/man1/python.1.gz

-可以知道python在/usr/bin目录中

[root@root ~]# cd /usr/bin/
[root@root bin]# ll python*
lrwxrwxrwx. 1 root root    7 2月   7 09:30 python -> python2
lrwxrwxrwx. 1 root root    9 2月   7 09:30 python2 -> python2.7
-rwxr-xr-x. 1 root root 7136 8月   4 2017 python2.7

-可以看到，python指向的是python2，python2指向的是python2.7，因此我们可以装个python3，然后将python指向python3，然后python2指向python2.7，那么两个版本的python就能共存了。


2.因为我们要安装python3，所以要先安装相关包，用于下载编译python3：
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make 
运行了以上命令以后，就安装了编译python3所用到的相关依赖


3.用wget下载python3的源码包
wget https://www.python.org/ftp/python/3.7.5/Python-3.7.5.tar.xz


4.编译python3源码包
#解压
xz -d Python-3.6.4.tar.xz
tar -xf Python-3.6.4.tar
 
#进入解压后的目录，依次执行下面命令进行手动编译
cd Python-3.6.4
./configure prefix=/usr/local/python3
make && make install
 
# 如果出现can't decompress data; zlib not available这个错误，则需要安装相关库
#安装依赖zlib、zlib-devel
yum install zlib zlib
yum install zlib zlib-devel

-如果最后没提示出错，就代表正确安装了，在/usr/local/目录下就会有python3目录


5.添加软链接
#将原来的链接备份
mv /usr/bin/python /usr/bin/python.bak
 
#添加python3的软链接
ln -s /usr/local/python3/bin/python3.7 /usr/bin/python


6.更改yum配置，因为其要用到python2才能执行，否则会导致yum不能正常使用
vi /usr/bin/yum
把#! /usr/bin/python修改为#! /usr/bin/python2
 
vi /usr/libexec/urlgrabber-ext-down
把#! /usr/bin/python 修改为#! /usr/bin/python2


7.查看链接情况：
[root@root bin]# ll -a python*
lrwxrwxrwx. 1 root root   33 Oct 21 12:30 python -> /usr/local/python3Dir/bin/python3
lrwxrwxrwx. 1 root root    9 Oct 19 23:55 python2 -> python2.7
-rwxr-xr-x. 1 root root 7136 Aug  4 08:40 python2.7
lrwxrwxrwx. 1 root root    7 Oct 19 23:55 python.bak -> python2


8.然后查看当前的python版本：
[root@root bin]# python -V
Python 3.7.5
