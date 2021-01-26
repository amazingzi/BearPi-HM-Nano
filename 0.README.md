# cat1-Learn
学习我的cat1板子
一天 周一 小雪

一、使用Windows远程登录Linux，以MobaXterm工具为例：

==================================================================
二、将Linux shell改为bash
  查看shell是否为bash，在终端运行如下命令
    ls -l /bin/sh
  方法一：在终端运行如下命令，然后选择 no。
    sudo dpkg-reconfigure dash
  方法二：先删除sh，再创建软链接。
    rm -rf /bin/sh
    sudo ln -s /bin/bash /bin/sh
    
==================================================================
# 安装Python环境
1、需使用python3.7以上版本
  sudo apt-get install python3.8
2、若系统存在python3，通过"python3 --version"指令查询版本，若不是3.7以上版本，则还需要执行下面指令
 "cd /usr/bin && sudo rm python3 && sudo ln -s /usr/bin/python3.8 python3 && python3 --version"
3、安装Python包管理工具
  sudo apt-get install python3-setuptools python3-pip -y
4、安装python模块setuptools，运行
  "pip3 install setuptools"
5、运行“sudo pip3 install kconfiglib”
6、安装pycryptodome。运行
  “sudo pip3 install pycryptodome”
7、安装six。运行
  "sudo pip3 install six --upgrade --ignore-installed six"
8、安装ecdsa。运行
  "sudo pip3 install ecdsa"
  
  ------------------------------------------------------------------
 为python建立软链接
 sudo rm /usr/bin/python
 sudo ln -s pyhon3.8 /usr/bin/python
 要python3 --version为3.8
 
==================================================================
# 安装Scons
安装SCons安装包
  sudo apt-get install scons -y 
查看版本
  scons -v
要求3.0.4以上版本
下载源码包（下载路径：“ https://scons.org/pages/download.html ”，推荐SCons版本是3.0.4+）。
解压源码包到任意目录。
安装源码包：进入解压目录，运行命令“sudo python3 setup.py install”（需root/sudo权限安装），等待安装完毕 
1、在服务器创建文件夹存放工具包（名叫tools）
  mkdir tools
2、讲下载好的文件直接拖至ssh文件夹内
  cd tools/
  tar -vxf scons-4.0.1.tar.gz
  cd SCons-4.0.1/
  sudo python3 setup.py install
  
==================================================================
# 安装编译工具环境

1、安装gn
-------------
http://tools.harmonyos.com/mirrors/gn/1523/linux/gn.1523.tar

2、安装ninja
-------------
http://tools.harmonyos.com/mirrors/ninja/1.9.0/linux/ninja.1.9.0.tar

3、安装gcc_riscv32
------------------
http://tools.harmonyos.com/mirrors/gcc_riscv32/7.3.0/linux/gcc_riscv32-linux-7.3.0.tar.gz


将下载好的所有软件包放置服务器文件夹内
-------------------------------------
tar -vxf gn.1523.tar -C ~/
tar -vxf ninja.1.9.0.tar -C ~/
tar -vxf gcc_riscv32-linux-7.3.0.tar.gz -C ~/


设置环境变量
-------------

**********************
如果编译出错，大概率软连接有误
*  sudo rm -rf /usr/bin/python3
*  sudo rm -rf /usr/bin/python
*  sudo ln -s /usr/bin/python3.8 /usr/bin/python3
*  sudo ln -s /usr/bin/python3.8 /usr/bin/python
-------------
如果：
ModuleNotFoundError: No module named 'apt_pkg'

sudo apt-get remove --purge python-apt
sudo apt-get install python-apt -f
cd /usr/lib/python3/dist-packages/
sudo cp apt_pkg.cpython-36m-x86_64-linux-gnu.so apt_pkg.cpython-38m-x86_64-linux-gnu.so

1、cd /usr/lib/python3/dist-packages/
2、执行软连接
ln -s apt_pkg.cpython-36m-x86_64-linux-gnu.so apt_pkg.so
如果apt_pkg.so已存在，则使用：
ln -fs apt_pkg.cpython-36m-x86_64-linux-gnu.so apt_pkg.so 

安装vim
----------
查看版本：lsb_release -a
sudo apt-get install vim-gtk

**********************

vim ~/.bashrc
将以下命令拷贝到文件底部
export PATH=~/gn:$PATH
export PATH=~/ninja:$PATH
export PATH=~/gcc_riscv32/bin:$PATH
生效环境变量
source~/.bashrc
Shell命令行中输入“riscv32-unknown-elf-gcc -v”，如果能正确显示编译器版本号，表明编译器安装成功。
返回：
gcc version 7.3.0 (GCC) 
成功
tar -vxf ninja.1.9.0.tar -C~/

****************************
https://gitee.com/bearpi/bearpi-hm_nano/blob/master/applications/BearPi/BearPi-HM_Nano/docs/quick-start/BearPi-HM_Nano%E5%BC%80%E5%8F%91%E6%90%AD%E5%BB%BA%E7%8E%AF%E5%A2%83.md#




































