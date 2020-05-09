# 笔记

## 一、命令

### 1、apt相关

apt-cache和apt-get是apt包的管理工具，他们根据/etc/apt/sources.list里的软件源地址列表搜索目标软件、并通过维护本地软件包列表来安装和卸载软件。

查看本机是否安装软件： whereis package_name  或者 which package_name

1.搜索软件

```
sudo  apt-cache  search  package_name
```

其中还可以使用正则表达式 sudo apt-cache search sof* 这样就可以搜索到源上面所有以sof开头的软件包。

2.查看软件包信息

```
sudo apt-cache show package_name
```

3.查看软件包依赖关系

```
sudo apt-cache show depends package_name
```

4.查看每个软件包的简要信息

```
sudo apt-cache dump
```

5.安装软件

```
sudo apt-get install  package_name
```

6.更新已安装的软件包

```
sudo apt-get  upgrade
```

7.更新软件包列表

```
sudo apt-get update
```

8.卸载一个软件包但是保留相关的配置文件

```
sudo apt-get remove package_name
```

9.卸载一个软件包同时删除配置文件

```
apt-get -purge remove package_name
```

10.删除软件包的备份

```
apt-get clean
```

下面我们列出 Ubuntu 16.04 LTS 中使用 ATP 命令与老版本 Ubuntu 中软件包管理的用法对比：

| apt 命令         | 取代的命令           | 命令的功能                     |
| ---------------- | -------------------- | ------------------------------ |
| apt install      | apt-get install      | 安装软件包                     |
| apt remove       | apt-get remove       | 移除软件包                     |
| apt purge        | apt-get purge        | 移除软件包及配置文件           |
| apt update       | apt-get update       | 刷新存储库索引                 |
| apt upgrade      | apt-get upgrade      | 升级所有可升级的软件包         |
| apt autoremove   | apt-get autoremove   | 自动删除不需要的包             |
| apt full-upgrade | apt-get dist-upgrade | 在升级软件包时自动处理依赖关系 |
| apt search       | apt-cache search     | 搜索应用程序                   |
| apt show         | apt-cache show       | 显示装细节                     |

11.依赖安装

```
apt-get build-dep package
```

```
如果本地依赖有变或者软件包不在源里，也可以用 sudo mk-build-deps -i ukui-settings-daemon/debian/control
```

### 2、dpkg相关

**dpkg命令**是Debian Linux系统用来安装、创建和管理软件包的实用工具。

```
dpkg -i package.deb        #安装包
dpkg -r package            #删除包
dpkg -P package            #删除包（包括配置文件）
dpkg -L package            #列出与该包关联的文件
dpkg -l package            #显示该包的版本
dpkg --unpack package.deb  #解开deb包的内容
dpkg -S keyword            #搜索所属的包内容
dpkg -l                    #列出当前已安装的包
dpkg -c package.deb        #列出deb包的内容
dpkg --configure package   #配置包
```

### 3、其他命令

#### 3.1axel多线程下载

```
apt-get install axel
```

 由于 CentOS 源里默认没有包含 axel，我们需要安装 EPEL 才能通过 yum 进行安装。

```
yum install epel-release
yum install axel
```

使用

    其中[options]可以包括如下参数：
    --max-speed=x , -s x 最高速度x 
    --num-connections=x , -n x 连接数x 
    --output=f , -o f 下载为本地文件f 
    --search[=x] , -S [x] 搜索镜像 
    --header=x , -H x 添加头文件字符串x（指定 HTTP header） 
    --user-agent=x , -U x 设置用户代理（指定 HTTP user agent） 
    --no-proxy ， -N 不使用代理服务器 --quiet ， -q 静默模式 
    --verbose ，-v 更多状态信息 
    --alternate ， -a Alternate progress indicator 
    --help ，-h 帮助 
    --version ，-V 版本信息

实例

    以 8 线程下载我 DD WIN 安装包，并保存文件至 / tmp 文件夹

axel -n 8 -o /tmp/ http://185.164.138.19:18910/bt/iso/win2016_vol_cn_noname%40007.gz

axel https://mirrors.edge.kernel.org/pub/linux/kernel/v2.6/linux-2.6.29.tar.bz2

#### 3.2 xdg-email

xdg-email打开用户首选的电子邮件编写器，以便向其发送邮件。

#### 3.3 git push origin HEAD:refs/for/master

#### 3.4 koji使用

koji使用

配置：

1./etc/koji.conf 配置10.1.82.10为提交的目的机器

[koji]

server = http://10.1.82.10/kojihub

weburl = http://10.1.82.10/koji

topurl = https://10.1.82.10/

2.测试提交

koji --user xxx --password xxx build tagname xxx.src.rpm --nowait --scratch

注意：一定要加上--scratch；测试提交的目的在于验证是否能顺利提交

#### 3.5 fedora下载

## https://archives.fedoraproject.org/pub/archive/fedora/linux/releases/

## 二、git使用

### 1、Git客户端配置

git config --system core.fileMode false #禁止Git对文件权限的跟踪

git config --system core.quotepath false #输出中文文件名显示问题

git config --system color.ui true    #开启颜色显示

git config --global user.name "kobe"  #配置Git用户名

git config --global user.email "xiangli@ubuntukylin.com" #配置mail

git config –l  #查看Git设置

### 2、配置SSH key

ssh-keygen -t rsa -C “你的邮箱@ubuntukylin.com"

cat ~/.ssh/id_rsa.pub

cat ~/.ssh/id_rsa.pub > /dev/clipboard #将公钥复制到剪贴板

### 3、常用命令

创建一个库：git init

克隆一个库：git clone git://git.kernel.org/scm/git/git.git

提交：git add/git commit

获取信息：git help/git status/git diff/git log/git show （显示改动情况）

### 4、关键的Git文件或目录

~/.gitconfig

.git#在库的顶级目录当中，包含项目的所有对象、提交记录、配置

.gitignore#记录要忽略的文件

### 5、示例

mkdir testproject

cd testproject

git init

touch README

git add README

git commit -m 'first commit'

git remote add origin git@192.168.31.15:test.git

git push -u origin master

git clone git@192.168. 31.15:test.git（获取现有项目）

## 三、Deb打包

### 1、编译环境准备

sudo apt-get install build-essential debhelper make autoconf automake dpkg-dev fakeroot pbuilder gnupg

### 2、debian文件解析

- #### 1、安装前执行脚本debian/preinst

  Debian软件包(".deb")解压前执行的脚本，为正在被升级的包停止相关服务，直到升级或安装完成(成功后执行 'postinst' 脚本)

  \#!/bin/sh

  if [ "$1" = "upgrade" ] || [ "$1" = "install" ];then

  需要执行的脚本

  fi            

- #### 2、安装后执行脚本debian/postinst

  主要完成软件包(".deb")安装完成后所需的配置工作。通常，postinst 脚本要求用户输入， 和/或警告用户如果接受默认值，应该记得按要求返回重新配置这个软件。一个软件包安装或升级完成后，postinst 脚本驱动命令，启动或重起相应的服务。

  \#!/bin/sh

  set -e

  if [ "$1" = "configure" ]; then

  ldconfig

  fi           

- #### 3、卸载前执行脚本debian/prerm

  停止一个软件包的相关进程，要卸载软件包的相关文件前执行。

  if [ "$1" = "remove" -o "$1" = "deconfigure" ]; then

  需要执行的脚本

  fi

- #### 4、卸载后执行脚本debian/postrm

  修改相关文件或连接，和/或卸载软件包所创建的文件。

  \#!/bin/sh

  if [ "$1" = "upgrade" ] ; then

  需要执行的脚本

  elif [ "$1" = "remove" ] || [ "$1" = "purge" ] ; then

  需要执行的脚本

  fi        

- #### 5、debian/changelog

  changelog提供版本修改信息，帮助下载软件包的人了解软件包中是否有他们需要知道的信息，决定着生成的deb包的版本号。格式如下：

  package (version) distribution(s); urgency=urgency

  [optional blank line(s), stripped]

  \* change details

  more change details

  [blank line(s), included in output of dpkg-parsechangelog]

  \* even more change details

  [optional blank line(s), stripped]

  -- maintainer name [two spaces]  date

  （注：date可以使用命令date -R获取）

- #### 6、debian/control

  ​       control决定着deb包的包名、编译依赖和运行依赖。用dh_make生成的默认control文件格式如下(行号是为了添加解释，我们在这里特意加入的)：

  1 Source: uk-deb-demo

  2 Section: unknown

  3 Priority: extra

  4 Maintainer: Kobe Lee 

  5 Build-Depends: debhelper (>=9)

  6 Standards-Version: 3.9.5

  7 Homepage: <insert the upstream URL, if relevant>

  8

  9 Package: uk-deb-demo

  10 Architecture: any

  11 Depends: ${shlibs:Depends}, ${misc:Depends}

  12 Description: <insert up to 60 chars description>

  13 <insert long description, indented with spaces>

  1-7行是源码的control信息，9-13是二进制包的control信息。

  第 1 行是源代码包的名称。

  第 2 行是该源码包要进入发行版中的分类。

  第 3 行是描述用户安装该包的重要程度。由于这是一个常规优先级的软件，并不与其他软件包冲突，我们将优先级改为 optional。

  每个软件包都有一个维护者指定的优先级，用于包管理系统。这些优先级是：

  必须的(Required)：系统运转所必须的软件包。包括修复系统缺陷所必须的所有工具。不能删除这些软件包，否则系统可能会崩溃，且甚至有可能无法用 dpkg 恢复。仅有这类包的系统是不可用的，但是它为系统管理员启动系统安装其它软件提供足够的功能。

  重要的(Important)：在任何类 Unix 系统上均安装有该级别软件包。没有这类包，其它的包无法在系统上正常运转或使用，Emacs，X11，TeX 等大型应用程序不在此列。此类包构成基本系统。

  一般的(Standard)：Linux 系统里的一般软件包，构成小型字符系统。这是用户什么也不选也会默认安装的软件包. 不包括大型软件, 但是 Emacs(与其说它是一个应用软件,不如说它是基础构件)一小部分 TeX 和 LaTeX(不支持X)除外。

  可选的(Optional)：软件包包含了所有的你想要安装的文件，如果你一开始不知道它是什么。或者没有特殊的需要。这包括 X11，所有的 TeX 和许多应用程序。

  额外的(Extra)：这类包不是与其它高优先级的软件冲突，只有知道它的用途才可能对你有用，就是因为特别的原因而不能进入"可选"优先级。

  第 4 行是源码维护者的名字和邮箱。

  第 5 行是源码的编译依赖。

  第 6 行是此软件包所依据的“Debian Policy Manual” 标准版本号。

  第 7 行是源码的主页。

  第 9 行是二进制软件包的名称。通常情况下与源代码包相同，但不是必须的。

  第 10 行是目标机架构，指明二进制包的类型。如果你的软件包是平台独立的(例如一个 shell 或 Perl 脚本，或一些文档)，将这项改变为 all，否则写成any。

  第 11 行显示了 Debian 软件包系统中最强大的特性之一。每个软件包都可以和其他软件包有各种不同的关系。除  Depends 外，还有 Recommends、Suggests、Pre-Depends、Breaks、Conflicts、Provides 和 Replaces。

  第 12 行是软件的简短描述。

  第 13 行是软件的详细描述。

- #### 7、debian/compat

  定义兼容级别，Ubuntu Kylin下保持默认值即可。

- #### 8、debian/copyright

  包含源码的版权和许可，dh_make 可以给出一个 copyright 文件的模板。

- #### 9、debian/install

  一些文件可以通过该install脚本，在软件安装时安装到系统的指定路径下。在示例代码中我们使用的是setup.py文件来完成该工作的，install文件可以参考优客天气源码，源码地址：https://code.launchpad.net/~ubuntukylin-members/indicator-china-weather/trunk

- #### 10、debian/links

  创建额外的符号链接。

- #### 11、debian/rules

  rules文件本质上是一个Makefile文件，这个Makefile文件定义了创建deb格式软件包的规则。打包工具按照rules文件指定的规则，完成编译，将软件安装到临时安装目录，清理编译目录等操作，并依据安装到临时目录的文件来生成deb格式的软件包。

  dh_make 会生成一个使用 dh 命令的非常简单但非常强大的默认的 rules 文件：

  \#!/usr/bin/make -f

  \# -*- makefile -*-

  

  \# Uncomment this to turn on verbose mode.

  \#export DH_VERBOSE=1

  

  %:

  dh $@

  一般没有特殊要求的情况下，源码包使用默认配置就可以了，这里uk-deb-demo示例就是采用默认配置。

  准备好debian目录和源码后，就可以用命令dpkg-buildpackage -rfakeroot -b来编包了。Debian打包更多详情请访问：
  http://www.debian.org/doc/manuals/maint-guide/dreq.zh-cn.html     

- #### 12、Deb包示例：

  将源码uk-deb-demo-1.0.0.tar.gz解压，进入解压源码文件夹uk-deb-demo-1.0.0（uk-deb-demo-1.0.0.tar.gz和uk-deb-demo-1.0.0文件夹在同一级目录），打开终端，执行：

  $ dh_make -f ../uk-deb-demo-1.0.0.tar.gz

  操作截图如下：![img](https://www.ubuntukylin.com/ukd/home/images/01.png)          操作完成后，在源码文件夹uk-deb-demo-1.0.0下会自动生成了debian文件夹，保留截图所示的文件和文件夹，其他文件可以根据项目需要增减，如图：           ![img](https://www.ubuntukylin.com/ukd/home/images/02.png)          修改修改changelog、control和copyright，详情见示例源码文件夹中的uk-deb-demo-debian，终端执行：

  $ dpkg-buildpackage -rfakeroot -b

  至此，deb包已经生产，可以终端执行命令进行安装：

  $ sudo dpkg -i uk-deb-demo_1.0.0-0ubuntu1_all.deb

  安装完成后，终端运行二进制则可以显示界面了。

  $ uk-deb-demo 
  
  

## 四、Linux下的常用的打包和解压缩命令

各个不同后缀的含义如下：

    .tar打包格式：tar程序打包的数据，并没有压缩过
    .z格式：compress程序压缩的文件
    .gz压缩格式：gzip程序压缩的文件 .bz2压缩格式：bzip2程序压缩的文件
    .tar.gz打包压缩：tar程序打包的文件，其中并且经过 gzip 的压缩
    .tar.bz2打包压缩：tar程序打包的文件，其中并且经过 bzip2 的压缩

.tar

压缩：tar cvf FileName.tar FileName

解压：tar xvf FileName.tar

---------------------------------------------

.gz

解压1：gunzip FileName.gz

解压2：gzip -d FileName.gz

压缩：gzip FileName .tar.gz

解压：tar zxvf FileName.tar.gz

压缩：tar zcvf FileName.tar.gz DirName

---------------------------------------------

.bz2

解压1：bzip2 -d FileName.bz2

解压2：bunzip2 FileName.bz2

压缩： bzip2 -z FileName .tar.bz2

解压：tar jxvf FileName.tar.bz2

压缩：tar jcvf FileName.tar.bz2 DirName

---------------------------------------------

.bz 解压1：bzip2 -d FileName.bz

解压2：bunzip2 FileName.bz

压缩：未知 .tar.bz

解压：tar jxvf FileName.tar.bz

压缩：未知

---------------------------------------------

.Z

解压：uncompress FileName.Z

压缩：compress FileName

.tar.Z

解压：tar Zxvf FileName.tar.Z

压缩：tar Zcvf FileName.tar.Z DirName

---------------------------------------------

.tgz

解压：tar zxvf FileName.tgz

压缩：未知 .tar.tgz

解压：tar zxvf FileName.tar.tgz

压缩：tar zcvf FileName.tar.tgz FileName

---------------------------------------------

.zip

解压：unzip FileName.zip

压缩：zip FileName.zip DirName

---------------------------------------------

.rar

解压：rar a FileName.rar

压缩：rar e FileName.rar

 tar是打包命令，比较常见，下面给出他的不同参数的含义

    -c: 建立压缩档案
    -x：解压
    -t：查看内容
    -r：向压缩归档文件末尾追加文件
    -u：更新原压缩包中的文件

 












这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但这五个命令只能用其中一个。

 

下面的参数是根据需要在压缩或解压档案时可选的。

    -z：有gzip属性的
    -j：有bz2属性的
    -z：有compress属性的
    -v：显示所有过程
    -o：将文件解开到标准输出

-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名，并且是必须的。

下面给出一些例子

    tar -cf FileName.tar DirName：这条命令是将DirName的文件夹打成一个名为FileName.tar的包。-c是表示产生新的包，-f指定包的文件名；
     
    tar -cf FileName.tar *.jpg：这条命令是将所有.jpg的文件打成一个名为FileName.tar的包；
     
    tar -rf FileName.tar *.gif：这条命令是将所有.gif的文件增加到FileName.tar的包里面去。-r是表示增加文件的意思；
     
    tar -uf FileName.tar logo.gif：这条命令是更新原来tar包FileName.tar中logo.gif文件，-u是表示更新文件的意思；
     
    tar -tf FileName.tar：这条命令是列出FileName.tar包中所有文件，-t是列出文件的意思；
     
    tar -xf FileName.tar：这条命令是解出FileName.tar包中所有文件，-x是解开的意思。
# C++笔记

## 一、C++ 存储类

### 1、auto 存储类

自 C++ 11 以来，**auto** 关键字用于两种情况：声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符。

C++98标准中auto关键字用于自动变量的声明，但由于使用极少且多余，在C++11中已删除这一用法。

根据初始化表达式自动推断被声明的变量的类型，如：

auto f=3.14;      //double 

auto s("hello");  //const char* 

auto z = new auto(9); // int* 

auto x1 = 5, x2 = 5.0, x3='r';//错误，必须是初始化为同一类型

### 2、register 存储类

**register** 存储类用于定义存储在寄存器中而不是 RAM 中的局部变量。这意味着变量的最大尺寸等于寄存器的大小（通常是一个词），且不能对它应用一元的 '&' 运算符（因为它没有内存位置）。

register int  miles;

寄存器只用于需要快速访问的变量，比如计数器。还应注意的是，定义 'register' 并不意味着变量将被存储在寄存器中，它意味着变量可能存储在寄存器中，这取决于硬件和实现的限制。

### 3、static 存储类

**static** 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用 static 修饰局部变量可以在函数调用之间保持局部变量的值。

static 修饰符也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。

在 C++ 中，当 static 用在类数据成员上时，会导致仅有一个该成员的副本被类的所有对象共享。

### 4、extern 存储类

**extern** 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的。当您使用 'extern' 时，对于无法初始化的变量，会把变量名指向一个之前定义过的存储位置。

当您有多个文件且定义了一个可以在其他文件中使用的全局变量或函数时，可以在其他文件中使用 *extern* 来得到已定义的变量或函数的引用。可以这么理解，*extern* 是用来在另一个文件中声明一个全局变量或函数。

### 5、mutable 存储类

**mutable** 说明符仅适用于类的对象。它允许对象的成员替代常量。也就是说，mutable 成员可以通过 const 成员函数修改。

### 6、thread_local 存储类

使用 thread_local 说明符声明的变量仅可在它在其上创建的线程上访问。 变量在创建线程时创建，并在销毁线程时销毁。 每个线程都有其自己的变量副本。

thread_local 说明符可以与 static 或 extern 合并。

可以将 thread_local 仅应用于数据声明和定义，thread_local 不能用于函数声明或定义。

## 二、C++函数

### 1、函数参数

如果函数要使用参数，则必须声明接受参数值的变量。这些变量称为函数的**形式参数**。

形式参数就像函数内的其他局部变量，在进入函数时被创建，退出函数时被销毁。

当调用函数时，有三种向函数传递参数的方式：

| 调用类型                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [传值调用](https://www.runoob.com/cplusplus/cpp-function-call-by-value.html) | 该方法把参数的实际值复制给函数的形式参数。在这种情况下，修改函数内的形式参数对实际参数没有影响。 |
| [指针调用](https://www.runoob.com/cplusplus/cpp-function-call-by-pointer.html) | 该方法把参数的地址复制给形式参数。在函数内，该地址用于访问调用中要用到的实际参数。这意味着，修改形式参数会影响实际参数。 |
| [引用调用](https://www.runoob.com/cplusplus/cpp-function-call-by-reference.html) | 该方法把参数的引用复制给形式参数。在函数内，该引用用于访问调用中要用到的实际参数。这意味着，修改形式参数会影响实际参数。 |

默认情况下，C++ 使用**传值调用**来传递参数。一般来说，这意味着函数内的代码不能改变用于调用函数的参数。之前提到的实例，调用 max() 函数时，使用了相同的方法。

### 2、参数的默认值

当您定义一个函数，您可以为参数列表中后边的每一个参数指定默认值。当调用函数时，如果实际参数的值留空，则使用这个默认值。

这是通过在函数定义中使用赋值运算符来为参数赋值的。调用函数时，如果未传递参数的值，则会使用默认值，如果指定了值，则会忽略默认值。 

### 3、Lambda 函数与表达式

C++11 提供了对匿名函数的支持,称为 Lambda 函数(也叫 Lambda 表达式)。 

Lambda 表达式把函数看作对象。Lambda 表达式可以像对象一样使用，比如可以将它们赋给变量和作为参数传递，还可以像函数一样对其求值。

Lambda 表达式本质上与函数声明非常类似。Lambda 表达式具体形式如下:

```
[capture](parameters)->return-type{body}
```

例如：

```
[](int x, int y){ return x < y ; }
```

如果没有返回值可以表示为：

```
[capture](parameters){body}
```

例如：

```
[]{ ++global_x; } 
```

在一个更为复杂的例子中，返回类型可以被明确的指定如下：

```
[](int x, int y) -> int { int z = x + y; return z + x; }
```

本例中，一个临时的参数 z 被创建用来存储中间结果。如同一般的函数，z 的值不会保留到下一次该不具名函数再次被调用时。

如果 lambda 函数没有传回值（例如 void），其返回类型可被完全忽略。

在Lambda表达式内可以访问当前作用域的变量，这是Lambda表达式的闭包（Closure）行为。 与JavaScript闭包不同，C++变量传递有传值和传引用的区别。可以通过前面的[]来指定：

```
[]      // 沒有定义任何变量。使用未定义变量会引发错误。
[x, &y] // x以传值方式传入（默认），y以引用方式传入。
[&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
[=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。
[&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
[=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。
```

另外有一点需要注意。对于[=]或[&]的形式，lambda 表达式可以直接使用 this 指针。但是，对于[]的形式，如果要使用 this 指针，必须显式传入：

```
[this]() { this->someFunc(); }();
```

## 三、C++数字

通常，当我们需要用到数字时，我们会使用原始的数据类型，如 int、short、long、float 和 double 等等。这些用于数字的数据类型，其可能的值和数值范围，我们已经在 C++ 数据类型一章中讨论过。

### 1、C++ 数学运算

在 C++ 中，除了可以创建各种函数，还包含了各种有用的函数供您使用。这些函数写在标准 C 和 C++ 库中，叫做**内置**函数。您可以在程序中引用这些函数。

C++ 内置了丰富的数学函数，可对各种数字进行运算。下表列出了 C++ 中一些有用的内置的数学函数。

为了利用这些函数，您需要引用数学头文件  <cmath>。

| 序号 | 函数 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | **double cos(double);** 该函数返回弧度角（double 型）的余弦。 |
| 2    | **double sin(double);** 该函数返回弧度角（double 型）的正弦。 |
| 3    | **double tan(double);** 该函数返回弧度角（double 型）的正切。 |
| 4    | **double log(double);** 该函数返回参数的自然对数。           |
| 5    | **double pow(double, double);** 假设第一个参数为 x，第二个参数为 y，则该函数返回 x 的 y 次方。 |
| 6    | **double hypot(double, double);** 该函数返回两个参数的平方总和的平方根，也就是说，参数为一个直角三角形的两个直角边，函数会返回斜边的长度。 |
| 7    | **double sqrt(double);** 该函数返回参数的平方根。            |
| 8    | **int abs(int);** 该函数返回整数的绝对值。                   |
| 9    | **double fabs(double);** 该函数返回任意一个浮点数的绝对值。  |
| 10   | **double floor(double);** 该函数返回一个小于或等于传入参数的最大整数。 |

### 2、C++ 随机数

在许多情况下，需要生成随机数。关于随机数生成器，有两个相关的函数。一个是 **rand()**，该函数只返回一个伪随机数。生成随机数之前必须先调用 **srand()** 函数。

## 四、C++引用

引用变量是一个别名，也就是说，它是某个已存在变量的另一个名字。一旦把引用初始化为某个变量，就可以使用该引用名称或变量名称来指向变量。

### 1、C++ 引用 vs 指针

引用很容易与指针混淆，它们之间有三个主要的不同：

- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。

### 2、C++ 中创建引用

试想变量名称是变量附属在内存位置中的标签，您可以把引用当成是变量附属在内存位置中的第二个标签。因此，您可以通过原始变量名称或引用来访问变量的内容。例如：

```
int i = 17;
```

我们可以为 i 声明引用变量，如下所示：

```
int&  r = i;
double& s = d;
```

在这些声明中，& 读作**引用**。因此，第一个声明可以读作 "r 是一个初始化为 i 的整型引用"，第二个声明可以读作 "s 是一个初始化为 d 的 double 型引用"。 

## 五、C++日期和时间

C++ 标准库没有提供所谓的日期类型。C++ 继承了 C 语言用于日期和时间操作的结构和函数。为了使用日期和时间相关的函数和结构，需要在 C++ 程序中引用 <ctime> 头文件。

有四个与时间相关的类型：**clock_t、time_t、size_t** 和 **tm**。类型 clock_t、size_t 和 time_t 能够把系统时间和日期表示为某种整数。

结构类型 **tm** 把日期和时间以 C 结构的形式保存，tm 结构的定义如下：

struct tm { 

 int tm_sec;   // 秒，正常范围从 0 到 59，但允许至 61 

 int tm_min;   // 分，范围从 0 到 59 

 int tm_hour;  // 小时，范围从 0 到 23  

int tm_mday;  // 一月中的第几天，范围从 1 到 31  

int tm_mon;   // 月，范围从 0 到 11 

int tm_year;  // 自 1900 年起的年数  

int tm_wday;  // 一周中的第几天，范围从 0 到 6，从星期日算起  

int tm_yday;  // 一年中的第几天，范围从 0 到 365，从 1 月 1 日算起  int tm_isdst; // 夏令时

 }

下面是 C/C++ 中关于日期和时间的重要函数。所有这些函数都是 C/C++ 标准库的组成部分，您可以在 C++ 标准库中查看一下各个函数的细节。

| 序号 | 函数 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | [**time_t time(time_t \*time);**](https://www.runoob.com/cplusplus/c-function-time.html) 该函数返回系统的当前日历时间，自 1970 年 1 月 1 日以来经过的秒数。如果系统没有时间，则返回 .1。 |
| 2    | [**char \*ctime(const time_t \*time);**](https://www.runoob.com/cplusplus/c-function-ctime.html) 该返回一个表示当地时间的字符串指针，字符串形式 *day month year hours:minutes:seconds year\n\0*。 |
| 3    | [**struct tm \*localtime(const time_t \*time);**](https://www.runoob.com/cplusplus/c-function-localtime.html) 该函数返回一个指向表示本地时间的 **tm** 结构的指针。 |
| 4    | [**clock_t clock(void);**](https://www.runoob.com/cplusplus/c-function-clock.html) 该函数返回程序执行起（一般为程序的开头），处理器时钟所使用的时间。如果时间不可用，则返回 .1。 |
| 5    | [**char \* asctime ( const struct tm \* time );**](https://www.runoob.com/cplusplus/c-function-asctime.html) 该函数返回一个指向字符串的指针，字符串包含了 time 所指向结构中存储的信息，返回形式为：day month date hours:minutes:seconds year\n\0。 |
| 6    | [**struct tm \*gmtime(const time_t \*time);**](https://www.runoob.com/cplusplus/c-function-gmtime.html) 该函数返回一个指向 time 的指针，time 为 tm 结构，用协调世界时（UTC）也被称为格林尼治标准时间（GMT）表示。 |
| 7    | [**time_t mktime(struct tm \*time);**](https://www.runoob.com/cplusplus/c-function-mktime.html) 该函数返回日历时间，相当于 time 所指向结构中存储的时间。 |
| 8    | [**double difftime ( time_t time2, time_t time1 );**](https://www.runoob.com/cplusplus/c-function-difftime.html) 该函数返回 time1 和 time2 之间相差的秒数。 |
| 9    | [**size_t strftime();**](https://www.runoob.com/cplusplus/c-function-strftime.html) 该函数可用于格式化日期和时间为指定的格式。 |

### 使用结构 tm 格式化时间

**tm** 结构在 C/C++ 中处理日期和时间相关的操作时，显得尤为重要。tm 结构以 C 结构的形式保存日期和时间。大多数与时间相关的函数都使用了 tm 结构。下面的实例使用了 tm 结构和各种与日期和时间相关的函数。

在练习使用结构之前，需要对 C 结构有基本的了解，并懂得如何使用箭头 -> 运算符来访问结构成员。

## 六、类与对象

### 1、C++ 类定义

类定义是以关键字 **class** 开头，后跟类的名称。类的主体是包含在一对花括号中。类定义后必须跟着一个分号或一个声明列表。例如，我们使用关键字 **class** 定义 Box 数据类型，如下所示：

`class Box` 

`{`   

`public:      double length;   // 盒子的长度`      

`double breadth;  // 盒子的宽度`      

`double height;   // 盒子的高度` 

`};`

关键字 **public** 确定了类成员的访问属性。在类对象作用域内，公共成员在类的外部是可访问的。您也可以指定类的成员为 **private** 或 **protected**。 

### 2、定义 C++ 对象

类提供了对象的蓝图，所以基本上，对象是根据类来创建的。声明类的对象，就像声明基本类型的变量一样。下面的语句声明了类 Box 的两个对象：

`Box Box1;          // 声明 Box1，类型为 Box` 

`Box Box2;          // 声明 Box2，类型为 Box`

对象 Box1 和 Box2 都有它们各自的数据成员。

### 3、访问数据成员

类的对象的公共数据成员可以使用直接成员访问运算符 (.) 来访问。为了更好地理解这些概念，让我们尝试一下下面的实例：

`#include <iostream>`

`using namespace std;`

`class Box`

`{`

  `public:`

   `double length;  // 长度`

   `double breadth;  // 宽度`

   `double height;  // 高度`

`};`

`int main( )`

`{`

  `Box Box1;     // 声明 Box1，类型为 Box`

  `Box Box2;     // 声明 Box2，类型为 Box`

  `double volume = 0.0;   // 用于存储体积`

  `// box 1 详述`

  `Box1.height = 5.0;` 

  `Box1.length = 6.0;` 

  `Box1.breadth = 7.0;`

  `// box 2 详述`

  `Box2.height = 10.0;`

  `Box2.length = 12.0;`

  `Box2.breadth = 13.0;`

  `// box 1 的体积`

  `volume = Box1.height * Box1.length * Box1.breadth;`

  `cout << "Box1 的体积：" << volume <<endl;`

  `// box 2 的体积`

  `volume = Box2.height * Box2.length * Box2.breadth;`

  `cout << "Box2 的体积：" << volume <<endl;`

  `return 0;`

`}`

当上面的代码被编译和执行时，它会产生下列结果：

```
Box1 的体积：210
Box2 的体积：1560
```

需要注意的是，私有的成员和受保护的成员不能使用直接成员访问运算符 (.) 来直接访问。我们将在后续的教程中学习如何访问私有成员和受保护的成员。

### 4、类 和 对象详解

| 概念                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| [类成员函数]          | 类的成员函数是指那些把定义和原型写在类定义内部的函数，就像类定义中的其他变量一样。 |
| [类访问修饰符]        | 类成员可以被定义为 public、private 或 protected。默认情况下是定义为 private。 |
| [构造函数 & 析构函数] | 类的构造函数是一种特殊的函数，在创建一个新的对象时调用。类的析构函数也是一种特殊的函数，在删除所创建的对象时调用。 |
| [C++ 拷贝构造函数]    | 拷贝构造函数，是一种特殊的构造函数，它在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象。 |
| [C++ 友元函数]        | **友元函数**可以访问类的 private 和 protected 成员。         |
| [C++ 内联函数]        | 通过内联函数，编译器试图在调用函数的地方扩展函数体中的代码。 |
| [C++ 中的 this 指针]  | 每个对象都有一个特殊的指针 **this**，它指向对象本身。        |
| [C++ 中指向类的指针]  | 指向类的指针方式如同指向结构的指针。实际上，类可以看成是一个带有函数的结构。 |
| [C++ 类的静态成员]    | 类的数据成员和函数成员都可以被声明为静态的。                 |

### 5、类成员函数

类的成员函数是指那些把定义和原型写在类定义内部的函数，就像类定义中的其他变量一样。类成员函数是类的一个成员，它可以操作类的任意对象，可以访问对象中的所有成员。

让我们看看之前定义的类 Box，现在我们要使用成员函数来访问类的成员，而不是直接访问这些类的成员：

`class Box`

`{`

  `public:`

   `double length;     // 长度`

   `double breadth;     // 宽度`

   `double height;     // 高度`

   `double getVolume(void);// 返回体积`

`};`

成员函数可以定义在类定义内部，或者单独使用**范围解析运算符 ::** 来定义。在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 inline 标识符。所以您可以按照如下方式定义 **Volume()** 函数：

`class Box`

`{`

  `public:`

   `double length;    // 长度`

   `double breadth;   // 宽度`

   `double height;    // 高度`

   `double getVolume(void)`

   `{`

​     `return length * breadth * height;`

   `}`

`};`

您也可以在类的外部使用**范围解析运算符 ::** 定义该函数，如下所示：

`double Box::getVolume(void)`

`{`

  `return length * breadth * height;`

`}`

在这里，需要强调一点，在 :: 运算符之前必须使用类名。调用成员函数是在对象上使用点运算符（**.**），这样它就能操作与该对象相关的数据，如下所示：

`Box myBox;      // 创建一个对象`

`myBox.getVolume();  // 调用该对象的成员函数`

### 6、类访问修饰符

数据封装是面向对象编程的一个重要特点，它防止函数直接访问类类型的内部成员。类成员的访问限制是通过在类主体内部对各个区域标记 public、private、protected 来指定的。关键字 public、private、protected 称为访问修饰符。

一个类可以有多个 public、protected 或 private 标记区域。每个标记区域在下一个标记区域开始之前或者在遇到类主体结束右括号之前都是有效的。成员和类的默认访问修饰符是 private。

`class Base {`

  `public:` 

 `// 公有成员`

  `protected:`

 `// 受保护成员`

  `private:`

 `// 私有成员`

`};` 

#### 公有（public）成员

**公有**成员在程序中类的外部是可访问的。您可以不使用任何成员函数来设置和获取公有变量的值。

`#include <iostream>`

`using namespace std;`

`class Line`

`{`

  `public:`

   `double length;`

   `void setLength( double len );`

   `double getLength( void );`

`};`

`// 成员函数定义`

`double Line::getLength(void)`

`{`

  `return length ;`

`}`

`void Line::setLength( double len )`

`{`

  `length = len;`

`}`

`// 程序的主函数`

`int main( )`

`{`

  `Line line;`

  `// 设置长度`

  `line.setLength(6.0);` 

  `cout << "Length of line : " << line.getLength() <<endl;`

  `// 不使用成员函数设置长度`

  `line.length = 10.0; // OK: 因为 length 是公有的`

  `cout << "Length of line : " << line.length <<endl;`

  `return 0;`

`}`

#### 私有（private）成员

**私有**成员变量或函数在类的外部是不可访问的，甚至是不可查看的。只有类和友元函数可以访问私有成员。

默认情况下，类的所有成员都是私有的。例如在下面的类中，**width** 是一个私有成员，这意味着，如果您没有使用任何访问修饰符，类的成员将被假定为私有成员：

`class Box`

`{`

  `double width;`

  `public:`

   `double length;`

   `void setWidth( double wid );`

   `double getWidth( void );`

`};`

实际操作中，我们一般会在私有区域定义数据，在公有区域定义相关的函数，以便在类的外部也可以调用这些函数，如下所示：

`#include <iostream>` 

`using namespace std;` 

`class Box`

`{`

  `public:`

   `double length;`

   `void setWidth( double wid );`

   `double getWidth( void );`

  `private:`

   `double width;`

`};` 

`// 成员函数定义`

`double Box::getWidth(void)`

`{`

  `return width ;`

`}`

`void Box::setWidth( double wid )`

`{`

  `width = wid;`

`}` 

`// 程序的主函数`

`int main( )`

`{`

  `Box box;` 

  `// 不使用成员函数设置长度`

  `box.length = 10.0; // OK: 因为 length 是公有的`

  `cout << "Length of box : " << box.length <<endl;`

  `// 不使用成员函数设置宽度`

  `// box.width = 10.0; // Error: 因为 width 是私有的`

  `box.setWidth(10.0);  // 使用成员函数设置宽度`

  `cout << "Width of box : " << box.getWidth() <<endl;`

  `return 0;`

`}`

#### 保护（protected）成员

**保护**成员变量或函数与私有成员十分相似，但有一点不同，保护成员在派生类（即子类）中是可访问的。

在下一个章节中，您将学习到派生类和继承的知识。现在您可以看到下面的实例中，我们从父类 **Box** 派生了一个子类 **smallBox**。

下面的实例与前面的实例类似，在这里 **width** 成员可被派生类 smallBox 的任何成员函数访问。

`#include <iostream>`

`using namespace std;`

`class Box`

`{`

  `protected:`

   `double width;`

`};` 

`class SmallBox:Box // SmallBox 是派生类`

`{`

  `public:`

   `void setSmallWidth( double wid );`

   `double getSmallWidth( void );`

`};` 

`// 子类的成员函数`

`double SmallBox::getSmallWidth(void)`

`{`

  `return width ;`

`}` 

`void SmallBox::setSmallWidth( double wid )`

`{`

  `width = wid;`

`}` 

`// 程序的主函数`

`int main( )`

`{`

  `SmallBox box;`

  `// 使用成员函数设置宽度`

  `box.setSmallWidth(5.0);`

  `cout << "Width of box : "<< box.getSmallWidth() << endl;`

  `return 0;`

`}`

#### 继承中的特点

有public, protected, private三种继承方式，它们相应地改变了基类成员的访问属性。

- 1.**public 继承：**基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：public, protected, private
- 2.**protected 继承：**基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：protected, protected, private
- 3.**private 继承：**基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：private, private, private

但无论哪种继承方式，上面两点都没有改变：

- 1.private 成员只能被本类成员（类内）和友元访问，不能被派生类访问；
- 2.protected 成员可以被派生类访问。

##### public 继承

基类的public成员，在派生类中仍是public成员。

 基类的protected成员，在派生类中仍是protected可以被派生类访问。

基类的private成员不能被派生类访问。

类外不能访问protected成员

类外不能访问private成员

##### protected 继承

基类的public成员，在派生类中变成了protected，可以被派生类访问。

 基类的protected成员，在派生类中还是protected，可以被派生类访问。

基类的private成员不能被派生类访问。

protected成员不能在类外访问。

 private成员不能在类外访问。

##### private 继承

基类public成员,在派生类中变成了private,可以被派生类访问。

 基类的protected成员，在派生类中变成了private,可以被派生类访问。

基类的private成员不能被派生类访问。

protected成员不能在类外访问。

private成员不能在类外访问。

### 7、类的构造函数和析构函数

#### 类的构造函数

类的**构造函数**是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。

构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于为某些成员变量设置初始值。`#include <iostream>`

`using namespace std;`

`class Line`

`{`

  `public:`

   `void setLength( double len );`

   `double getLength( void );`

   `Line();  // 这是构造函数`

  `private:`

   `double length;`

`};`

`// 成员函数定义，包括构造函数`

`Line::Line(void)`

`{`

  `cout << "Object is being created" << endl;`

`}`

`void Line::setLength( double len )`

`{`

  `length = len;`

`}` 

`double Line::getLength( void )`

`{`

  `return length;`

`}`

`// 程序的主函数`

`int main( )`

`{`

  `Line line;`

  `// 设置长度`

  `line.setLength(6.0);` 

  `cout << "Length of line : " << line.getLength() <<endl;`

  `return 0;`

`}`

当上面的代码被编译和执行时，它会产生下列结果：

`Object is being created`

`Length of line : 6`

#### 带参数的构造函数

默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象赋初始值。

`#include <iostream>` 

`using namespace std;`

`class Line`

`{`

  `public:`

   `void setLength( double len );`

   `double getLength( void );`

   `Line(double len);  // 这是构造函数`

  `private:`

   `double length;`

`};`

`// 成员函数定义，包括构造函数`

`Line::Line( double len)`

`{`

  `cout << "Object is being created, length = " << len << endl;`

  `length = len;`

`}` 

`void Line::setLength( double len )`

`{`

  `length = len;`

`}`

`double Line::getLength( void )`

`{`

  `return length;`

`}`

`// 程序的主函数`

`int main( )`

`{`

  `Line line(10.0);`

  `// 获取默认设置的长度`

  `cout << "Length of line : " << line.getLength() <<endl;`

  `// 再次设置长度`

  `line.setLength(6.0);` 

  `cout << "Length of line : " << line.getLength() <<endl;`

  `return 0;`

`}`

当上面的代码被编译和执行时，它会产生下列结果：

```
Object is being created, length = 10
Length of line : 10
Length of line : 6
```

#### 使用初始化列表来初始化字段

使用初始化列表来初始化字段：

`Line::Line( double len): length(len)`

`{`

  `cout << "Object is being created, length = " << len << endl;`

`}`

上面的语法等同于如下语法：

`Line::Line( double len)`

`{`

  `length = len;`

  `cout << "Object is being created, length = " << len << endl;`

`}`

假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理地，您可以使用上面的语法，只需要在不同的字段使用逗号进行分隔，如下所示：

C::C( double a, double b, double c): X(a), Y(b), Z(c)

{

 ....

}

#### 类的析构函数

类的**析构函数**是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。

析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。

`#include <iostream>`

`using namespace std;` 

`class Line`

`{`

  `public:`

   `void setLength( double len );`

   `double getLength( void );`

   `Line();  // 这是构造函数声明`

   `~Line();  // 这是析构函数声明`

  `private:`

   `double length;`

`};`

`// 成员函数定义，包括构造函数`

`Line::Line(void)`

`{`

  `cout << "Object is being created" << endl;`

`}`

`Line::~Line(void)`

`{`

  `cout << "Object is being deleted" << endl;`

`}` 

`void Line::setLength( double len )`

`{`

  `length = len;`

`}` 

`double Line::getLength( void )`

`{`

  `return length;`

`}`

`// 程序的主函数`

`int main( )`

`{`

  `Line line;`

  `// 设置长度`

  `line.setLength(6.0);` 

  `cout << "Length of line : " << line.getLength() <<endl;`

  `return 0;`

`}`

当上面的代码被编译和执行时，它会产生下列结果：

```
Object is being created
Length of line : 6
Object is being deleted
```

### 8、拷贝构造函数

**拷贝构造函数**是一种特殊的构造函数，它在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象。拷贝构造函数通常用于：

- 通过使用另一个同类型的对象来初始化新创建的对象。
- 复制对象把它作为参数传递给函数。
- 复制对象，并从函数返回这个对象。

如果在类中没有定义拷贝构造函数，编译器会自行定义一个。如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。拷贝构造函数的最常见形式如下：

`classname (const classname &obj) {`

   `// 构造函数的主体`

 `}`

在这里，**obj** 是一个对象引用，该对象是用于初始化另一个对象的。

`#include <iostream>`

`using namespace std;` 

`class Line`

`{`

  `public:`

   `int getLength( void );`

   `Line( int len );       // 简单的构造函数`

   `Line( const Line &obj);    // 拷贝构造函数`

   `~Line();           // 析构函数`

  `private:`

   `int *ptr;`

`};` 

`// 成员函数定义，包括构造函数`

`Line::Line(int len)`

`{`

  `cout << "调用构造函数" << endl;`

  `// 为指针分配内存`

  `ptr = new int;`

  `*ptr = len;`

`}` 

`Line::Line(const Line &obj)`

`{`

  `cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;`

  `ptr = new int;`

  `*ptr = *obj.ptr; // 拷贝值`

`}` 

`Line::~Line(void)`

`{`

  `cout << "释放内存" << endl;`

  `delete ptr;`

`}`

`int Line::getLength( void )`

`{`

  `return *ptr;`

`}`

`` 

`void display(Line obj)`

`{`

  `cout << "line 大小 : " << obj.getLength() <<endl;`

`}`

`// 程序的主函数`

`int main( )`

`{`

  `Line line1(10);`

  `Line line2 = line1; // 这里也调用了拷贝构造函数`

  `display(line1);`

  `display(line2);` 

  `return 0;`

`}`

当上面的代码被编译和执行时，它会产生下列结果：

```
调用构造函数
调用拷贝构造函数并为指针 ptr 分配内存
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
释放内存
释放内存
```

### 9、友元函数

类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。

友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。

如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 **friend**，如下所示

`class Box`

`{`

  `double width;`

`public:`

  `double length;`

  `friend void printWidth( Box box );`

  `void setWidth( double wid );`

`};`

声明类 ClassTwo 的所有成员函数作为类 ClassOne 的友元，需要在类 ClassOne 的定义中放置如下声明：

```
friend class ClassTwo;
```

### 10、内联函数

C++ **内联函数**是通常与类一起使用。如果一个函数是内联的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方。

对内联函数进行任何修改，都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧的函数。

如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 **inline**，在调用函数之前需要对函数进行定义。如果已定义的函数多于一行，编译器会忽略 inline 限定符。

在类定义中的定义的函数都是内联函数，即使没有使用 **inline** 说明符。

### 11、this指针

在 C++ 中，每一个对象都能通过 **this** 指针来访问自己的地址。**this** 指针是所有成员函数的隐含参数。因此，在成员函数内部，它可以用来指向调用对象。

友元函数没有 **this** 指针，因为友元不是类的成员。只有成员函数才有 **this** 指针。

### 12、指向类的指针

一个指向 C++ 类的指针与指向结构的指针类似，访问指向类的指针的成员，需要使用成员访问运算符 **->**，就像访问指向结构的指针一样。与所有的指针一样，您必须在使用指针之前，对指针进行初始化。

### 13、类的静态成员

我们可以使用 **static** 关键字来把类成员定义为静态的。当我们声明类的成员为静态时，这意味着无论创建多少个类的对象，静态成员都只有一个副本。

静态成员在类的所有对象中是共享的。如果不存在其他的初始化语句，在创建第一个对象时，所有的静态数据都会被初始化为零。我们不能把静态成员的初始化放置在类的定义中，但是可以在类的外部通过使用范围解析运算符 **::** 来重新声明静态变量从而对它进行初始化。

#### 静态成员函数

如果把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。静态成员函数即使在类对象不存在的情况下也能被调用，**静态函数**只要使用类名加范围解析运算符 **::** 就可以访问。

静态成员函数只能访问静态成员数据、其他静态成员函数和类外部的其他函数。

静态成员函数有一个类范围，他们不能访问类的 this 指针。您可以使用静态成员函数来判断类的某些对象是否已被创建。

> **静态成员函数与普通成员函数的区别：**
>
> - 静态成员函数没有 this 指针，只能访问静态成员（包括静态成员变量和静态成员函数）。
> - 普通成员函数有 this 指针，可以访问类中的任意成员；而静态成员函数没有 this 指针。

## 七、继承

面向对象程序设计中最重要的一个概念是继承。继承允许我们依据另一个类来定义一个类，这使得创建和维护一个应用程序变得更容易。这样做，也达到了重用代码功能和提高执行效率的效果。

当创建一个类时，您不需要重新编写新的数据成员和成员函数，只需指定新建的类继承了一个已有的类的成员即可。这个已有的类称为**基类**，新建的类称为**派生类**。

### 1、基类 & 派生类

一个类可以派生自多个类，这意味着，它可以从多个基类继承数据和函数。定义一个派生类，我们使用一个类派生列表来指定基类。类派生列表以一个或多个基类命名，形式如下：

```
class derived-class: access-specifier base-class
```

其中，访问修饰符 access-specifier 是 **public、protected** 或 **private** 其中的一个，base-class 是之前定义过的某个类的名称。如果未使用访问修饰符 access-specifier，则默认为 private。

### 2、访问控制和继承

派生类可以访问基类中所有的非私有成员。因此基类成员如果不想被派生类的成员函数访问，则应在基类中声明为 private。

我们可以根据访问权限总结出不同的访问类型，如下所示：

| 访问     | public | protected | private |
| -------- | ------ | --------- | ------- |
| 同一个类 | yes    | yes       | yes     |
| 派生类   | yes    | yes       | no      |
| 外部的类 | yes    | no        | no      |

一个派生类继承了所有的基类方法，但下列情况除外：

- 基类的构造函数、析构函数和拷贝构造函数。
- 基类的重载运算符。
- 基类的友元函数。

### 3、继承类型

当一个类派生自基类，该基类可以被继承为 **public、protected** 或  **private** 几种类型。继承类型是通过上面讲解的访问修饰符 access-specifier 来指定的。

我们几乎不使用 **protected** 或  **private** 继承，通常使用 **public** 继承。当使用不同类型的继承时，遵循以下几个规则：

- **公有继承（public）：**当一个类派生自**公有**基类时，基类的**公有**成员也是派生类的**公有**成员，基类的**保护**成员也是派生类的**保护**成员，基类的**私有**成员不能直接被派生类访问，但是可以通过调用基类的**公有**和**保护**成员来访问。
- **保护继承（protected）：** 当一个类派生自**保护**基类时，基类的**公有**和**保护**成员将成为派生类的**保护**成员。
- **私有继承（private）：**当一个类派生自**私有**基类时，基类的**公有**和**保护**成员将成为派生类的**私有**成员。

### 4、多继承

多继承即一个子类可以有多个父类，它继承了多个父类的特性。

C++ 类可以从多个类继承成员，语法如下：

```
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```

## 八、重载运算符和重载函数

C++ 允许在同一作用域中的某个**函数**和**运算符**指定多个定义，分别称为**函数重载**和**运算符重载**。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同。

当您调用一个**重载函数**或**重载运算符**时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为**重载决策**。

### 1、C++ 中的函数重载

在同一个作用域内，可以声明几个功能类似的同名函数，但是这些同名函数的形式参数（指参数的个数、类型或者顺序）必须不同。您不能仅通过返回类型的不同来重载函数。

### 2、C++ 中的运算符重载

您可以重定义或重载大部分 C++ 内置的运算符。这样，您就能使用自定义类型的运算符。

重载的运算符是带有特殊名称的函数，函数名是由关键字 operator 和其后要重载的运算符符号构成的。与其他函数一样，重载运算符有一个返回类型和一个参数列表。

```
Box operator+(const Box&);
```

声明加法运算符用于把两个 Box 对象相加，返回最终的 Box 对象。大多数的重载运算符可被定义为普通的非成员函数或者被定义为类成员函数。如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数，如下所示：

```
Box operator+(const Box&, const Box&);
```

### 3、可重载运算符/不可重载运算符

下面是可重载的运算符列表：

| 双目算术运算符 | + (加)，-(减)，*(乘)，/(除)，% (取模)                        |
| -------------- | ------------------------------------------------------------ |
| 关系运算符     | ==(等于)，!= (不等于)，< (小于)，> (大于>，<=(小于等于)，>=(大于等于) |
| 逻辑运算符     | \|\|(逻辑或)，&&(逻辑与)，!(逻辑非)                          |
| 单目运算符     | + (正)，-(负)，*(指针)，&(取地址)                            |
| 自增自减运算符 | ++(自增)，--(自减)                                           |
| 位运算符       | \| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移) |
| 赋值运算符     | =, +=, -=, *=, /= , % = , &=, \|=, ^=, <<=, >>=              |
| 空间申请与释放 | new, delete, new[ ] , delete[]                               |
| 其他运算符     | ()(函数调用)，->(成员访问)，,(逗号)，[](下标)                |

下面是不可重载的运算符列表：

- .：成员访问运算符
- .*, ->*：成员指针访问运算符
- ::：域运算符
- sizeof：长度运算符
- ?:：条件运算符
- \#： 预处理符号

### 4、运算符重载实例

下面提供了各种运算符重载的实例，帮助您更好地理解重载的概念。

| 序号 | 运算符和实例                                                 |
| ---- | ------------------------------------------------------------ |
| 1    | [一元运算符重载](https://www.runoob.com/cplusplus/unary-operators-overloading.html) |
| 2    | [二元运算符重载](https://www.runoob.com/cplusplus/binary-operators-overloading.html) |
| 3    | [关系运算符重载](https://www.runoob.com/cplusplus/relational-operators-overloading.html) |
| 4    | [输入/输出运算符重载](https://www.runoob.com/cplusplus/input-output-operators-overloading.html) |
| 5    | [ ++ 和 -- 运算符重载](https://www.runoob.com/cplusplus/increment-decrement-operators-overloading.html) |
| 6    | [赋值运算符重载](https://www.runoob.com/cplusplus/assignment-operators-overloading.html) |
| 7    | [函数调用运算符 () 重载](https://www.runoob.com/cplusplus/function-call-operator-overloading.html) |
| 8    | [下标运算符[]重载](https://www.runoob.com/cplusplus/subscripting-operator-overloading.html) |
| 9    | [类成员访问运算符 -> 重载](https://www.runoob.com/cplusplus/class-member-access-operator-overloading.html) |

## 九、多态		

**多态**按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。

C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。

### 1、虚函数

**虚函数** 是在基类中使用关键字 **virtual** 声明的函数。在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。

我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为**动态链接**，或**后期绑定**。

### 2、纯虚函数

您可能想要在基类中定义虚函数，以便在派生类中重新定义该函数更好地适用于对象，但是您在基类中又不能对虚函数给出有意义的实现，这个时候就会用到纯虚函数。

`class Shape {`

  `protected:`

   `int width, height;`

  `public:`

   `Shape( int a=0, int b=0)`

   `{`

​     `width = a;`

​     `height = b;`

   `}`

   `// pure virtual function`

   `virtual int area() = 0;`

`};`

= 0 告诉编译器，函数没有主体，上面的虚函数是**纯虚函数**。

## 十、复合类型

### 1、数组

只有在定义时才能使用初始化，以后就不能使用了，也不能将一个数组赋值给另一个数组。

C++11将使用大括号的初始化(列表初始化)作为一种通用初始化方式，可用于所有类型。列表初始化数组时可以省略等号(=)，列表初始化禁止缩窄转换。

### 2、字符串

任何两个由空白(空格、制表符、换行符)分割的字符串常量都将自动拼接成一个。拼接时不会在被连接的字符串之间添加空格，第二个字符串的第一个字符将紧跟在第一个字符串的最后一个字符(不考虑\0)后面。第一个字符串的\0将被第二个字符串的第一个字符串取代。

sizeof运算符指出数组的长度，strlen()函数返回存储在数组中字符串的长度，strlen()只计算可见的字符。

cin使用空白(空白、制表符和换行符)来确定字符串的结束位置。

genline()和get()都读取一行输入，直到到达换行符。getline()将丢弃换行符，而get()将换行符保留在输入序列中。

getline()函数每次读取一行。它通过换行符来确定行尾，但不保存换行符。相反，在存储字符串时，它用空字符来替换换行符。

get()函数不再读取并丢弃换行符，而是将其留在输入队列中。

使用不带任何参数的cin.get()调用可读取下一个字符(即使是换行符)，因此可以用它来处理换行符。

当get()(不是getline())读取读取空行后将设置失效位(failbit)。这意味着接下来的输入将被阻断，可用cin.clear();来恢复输入。

当输入字符串比分配的空间长时，则getline()和get()将把余下的字符串留在输入队列中，而getline()还会设置失效位，并关闭后面的输入。

### 3、string类

要使用string类，须在程序中包含头文件string，string类隐藏了字符串的数组性质。

可以使用C-风格字符串来初始化string对象。

可以使用cin来将键盘输入存储到string对象。

可以使用cout来显示string对象。

可以使用数组表示法来访问存储在string对象中的字符。

string对象和字符数组之间的主要区别是，可以将string对象声明为简单变量，而不是数组。类设计让程序自动处理string的大小。

C++11也允许将列表初始化用于C-风格字符串和string对象。

不能将一个数组赋给另一个数组，但可以将一个string对象赋给另一给string对象。

string类简化了字符串合并操作。可以使用运算符+将两个string对象合并起来，还可以使用+=将字符串附加到string对象的末尾。

strlen()和size()都可以求字符串的字符数，但strlen()是一个常规函数，size()是一个类方法，只能通过其所属类的对象调用。

C++还有类型wchar_t，C++11还新增了char16_t和char32_t,分别用前缀L、u、U来表示。

wchar_t title[] = L"aaaas";

char16_t name[] = u"ssssssa";

char32_t car[] = U"saaaaaaa";

C++11还支持Unicode字符编码方案UTF-8，字符可能存储1-4个八位组。使用前缀u8来表示这种类型的字符串字面值。

C++11还新增的一种类型是原始(raw)字符串，在原始字符串中，字符表示的就是自己。如\n表示的就是\n不再是换行。在原始字符串将"(和)"用作定界符，并使用前缀R来表示原始字符串。

### 4、结构

在C++中，结构标记的用法与基本类型名相同。结构声明了一种新类型，在C++中省略struct不会出错。

struct people

{

​	char name[20];

​	int age;

​	flaot worth;

}

与数组一样，C++11也支持将列表初始化用于结构，且等号(=)是可选的。

people jack {"sd",18,30.1};

若大括号里未包含任何东西，各个成员都将被设置为零。

不允许缩窄转换。

可将结构做为参数传递给函数，也可以让函数返回一个结构。

还可以使用赋值运算符(=)将结构赋值给另一个同类型的结构。

可以同时完成定义结构和创建结构变量的工作。还可以声明没有名称的结构类型，方法是省略名称，同时定义一种结构类型和一个这种类型的变量。

可以创建元素为结构的数组。

### 5、共用体

共用体(union)是一种数据格式，它能够存储不同的数据类型，但只能同时存储其中的一种类型。

### 6、指针

使用常规变量时，值是指定的量，而地址为派生量。

处理存储数据的新策略刚好相反，将地址视为指定的量，而将值视为派生量。指针用于存储值的地址。指针名表示的是地址。*运算符被称为间接值或解除引用运算符，将其应用于指针，可以得到该地址处存储的值。

int *p_updates;

这表明*p_updates的类型为int。由于 * 运算符被用于指针，因此p_updates变量 本身必须是指针。我们说p_updates指向int类型，还说p_updates的类型是指向int的指针，或int*。可以这样说，p_updates是指针(地址),而*p_updates是int，而不是指针。

在C++中，int*是一种复合类型，是指向int的指针。

使用new和delete时，应遵循以下规则：

​	不要使用delete来释放不是new分配的内存。

​	不要使用delete释放同一个内存块两次。

​	如果使用new []为数组分配内存，则应使用delete []来释放。

​	如果使用new为一个实体分配内存，则应使用delte（没有方括号）来释放。

​	对空指针应用delete是安全的。

在cout和多数C++表达式中，char数组名、char指针以及用引号括起来的字符串常量都被解释为字符串的第一个字符的地址。

在将字符串读入程序时，应使用已分配的内存地址。该地址可以是数组名，也可以是使用new初始化过的指针。

应使用strcpy()或strncpy()，而不是赋值运算符来将字符串赋值给数组。

## 十一、文件输入/输出

#### 1、写入到文本文件

cout用于控制台输出的基本事实：

​	必须包含头文件iostream。

​	头文件iostream定义了一个用处理输出的ostream类。

​	头文件iostream声明了一个名为cout的ostream变量（对象）。

​	必须指明名称空间std。

​	可以结合cout和运算符<<来显示各种数据类型。

写入到文件的基本事实：

​	必须包含头文件fstream。

​	头文件fstream定义了一个用于处理输出的ofstream类。

​	需要声明一个或多个ofstream变量（对象）。

​	必须指明名称空间std。

​	需要将ofstream对象与文件关联起来。方法之一是使用open()方法。

​	使用完文件后，应使用close()方法将其关闭。

​	可结合ofstream对象和运算符<<来输出各种类型的数据。

```c++
ofstream outFile;

ofstream fout;

outFile.open("fish.txt");

char filename[50];

cin >> filename;

fout.open(filename);

double wt  = 125.8;

outFile << wt;
```

#### 2、读取文本文件内容

cin用于控制台输入的基本事实：

​	必须包含iostream。

​	头文件iostream定义了一个用于处理输入的istream类。

​	头文件iostream声明了一个名为cin的is他热爱吗变量（对象）。

​	必须指明名称空间std。

​	可以使用cin和get()方法来读取一个字符，使用cin和getline()来读取一行字符。

​	可以结合使用cin和eof()、fail()方法来判断输入是否成功。

​	对象cin本身被用作测试条件时，如果最后一个读取操作成功，它将被转换为布尔值true，否则被转换为false。

读取文件的基本事实：

​	必须包含头文件fstream。

​	头文件fstream定义了一个用于处理输入的ifstream类。

​	需要声明一个或多个ifstream变量（对象）。

​	必须指明名称空间std。

​	需要将ifstream对象与文件关联起来。方法之一是使用open()方法。

​	使用完文件后，应使用close()方法将其关闭。

​	可结合使用ifstream对象和运算符>>来读取各种类型的数据。

​	可以使用ifstream对象和get()方法来读取一个字符，使用ifstream对象和getline()来读取一行字符。

​	可以结合使用ifstream和eof()、fail()等方法来判断输入是否成功。

​	ifstream对象本身被用作测试条件时，如果最后一个读取操作成功，它将被转换为布尔值true，否则被转换为false。

```c++
ifstream inFile;

ifstream fin;

inFile.open("bowling.txt");

if(! inFile.is_open())

{

​	exit(EXIT_FAILURE)；

}

char filename[50];

cin >> filename;

fin.open(filename);

double wt;

inFile >> wt;

char line[81];

fin.getline(line,81);
```

## 十二、引用变量

C++新增了一种复合类型--引用变量。引用是已定义变量的别名。引用变量的主要作用是用作函数的形参。通过将引用变量用作参数，函数将使用原始数据，而不是其副本。

### 1、创建引用变量

和C++使用符号&符号来表示变量的地址。C++给&符号赋予了另一个含义，将其用来声明引用。

```c++
int rats;

int & rodents = rats;
```

此处的&运算符并不是地址运算符，而是将rodents的类型声明为int&，即指向int变量的引用。rats和rodents的值和地址都相同。

必须在声明引用时将其初始化，而不能像指针那样，先声明，后初始化。

引用更接近const指针，必须在创建时进行初始化，一旦与某个变量关联起来，就将一直效忠于它。

### 2、将引用用作函数参数

引用经常被用作函数参数，使得函数中的变量名成为调用程序的别名。这种传递参数称为引用传递。按引用传递允许被调用的函数能够访问调用函数中的变量。

### 3、引用的属性和特别之处

函数形参如果是引用变量，那么在被调用函数中修改了形参的值，实参的值也会被修改。若不想修改实参的值应该在函数原形和函数头中使用const。

```c++
double refcube(const double &ra);
```

如果实参和引用参数不匹配，C++将生成临时变量。当前，仅当参数为const引用时，C++才会这样做。

如果引用参数是const，将在下面两种情况下生成临时变量：

​	实参的类型正确，但不是左值。

​	实参类型不正确，但可以转换为正确的类型。

左值参数是可被引用的数据对象，例如，变量、数组元素、结构成员、引用和解引用的指针都是左值。

非左值包括字面常量（用引用括起来的字符串除外，它们由其地址表示）和包含多项的表达式。

在C语言中，左值最初指的是可出现在赋值语句左边的实体，但这是引入关键字const之前的情况。现在常规变量和const变量都可视为左值，因为可通过地址访问它们。但常规变量属于可修改的左值，而const变量属于不可修改的左值。

应尽可能将引用参数声明为const：

​	使用const可以避免无意中修改数据的编程错误；

​	使用const使函数能够处理const和非const实参，否则只能接受非const数据；

​	使用const引用使函数能够正确生成并使用临时变量。

### 4、对象、继承和引用

ostream和ofstream类凸显了引用的一个有趣属性。ofstream对象可以使用ostream类的方法，这使得文件输入/输出的格式与控制台输入/输出相同。使得能够将特性从一个类继承到另一个类的语言特性称之为继承。ostream是基类，而ofstream是派生类。派生类继承了基类的方法，这意味着ofstream对象可以使用基类的特性，如格式化方法precision()和setf()。

继承的另一个特征是，基类引用可以指向派生类对象，而无需强制类型转换。即可以定义一个接受基类引用作为参数的函数，调用函数时，可以将基类对象作为参数，也可以将派生类对象作为参数。

方法setf()设置各种格式化状态。方法调用setf(ios_base::fixed)将对象置于使用定点表示法模式；setf(ios_base::showpoint)将对象置于显示小数点的模式，即使小数部分为零。方法precision()指定显示多少位小数（假定对象处于定点模式下）。所有这些设置都将一直保持不变，直到再次调用相应的方法重新设置它们。方法width()设置下一次输出操作使用的字段宽度，这种设置只在显示下一个值时有效，然后将恢复到默认设置。默认的字段宽度为0.

方法setf()返回调用它之前有效的所有格式设置。ios_base::fmtflags是存储这种信息所需的数据类型名称。

### 5、何时使用引用参数

使用引用参数的主要原因有两个：

​	程序能够修改调用函数中的数据对象。

​	通过传递引用而不是整个数据对象，可以提高程序的运行速度。

当数据对象较大时，第二个原因最重要。这些也是使用指针参数的原因。因为引用参数实际上是基于指针的另一个接口。

对于使用传递的值而不作修改的函数：

​	如果数据对象很小，如内置数据类型或小型结构，则按值传递。

​	如果数据对象是数组，则使用指针，因为只是唯一的选择，并将指针声明为const的指针。

​	如果数据对象是较大的结构，则使用const指针或const引用，以提高程序的效率。这样可以节省复制结构所需的时间和空间。

​	如果数据对象是类对象，则应使用const引用。类设计的语义常常要求使用引用，这是C++新增这项特性的主要原因。因此，传递类对象的标准方式是按引用传递。

对于修改调用函数中数据的函数：

​	如果数据对象是内置数据类型，则使用指针。

如果数据对象是数组，则只能使用指针。

如果数据对象是结构，则使用引用或指针。

如果数据对象是类对象，则使用引用。

### 6、默认参数

默认参数指的是当函数调用中省略了实参时自动使用的一个值。

设置默认参数，必须通过函数原型。由于编译器通过查看原型来了解所使用的参数数目，因此函数原型也必须将可能的默认参数告知程序。方法是将值赋给原型中参数。

对于带参数列表的函数，必须从右向左添加默认值。也就是说，要为某个参数设置默认值，则必须为它右边的所有参数都提供默认值。

```c++
int harpo(int n, int m = 4, int j = 5);  //VALID

int harpo(int n, int m = 8, int j);  //INVALID

int harpo(int n = 1, int m = 3, int j = 9)  //VALID
```

实参按从左到右的顺序依次被赋值给相应的形参，而不能跳过任何参数。