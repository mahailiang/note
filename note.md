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

