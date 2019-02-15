---
layout:     post
title:      「Linux CentOS」 之 搭建Jenkins环境
subtitle:   「Linux CentOS」 之 搭建Jenkins环境
date:       2018-12-25
author:     JEEIT
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - Linux
    - Jenkins
---

> 正所谓前人栽树，后人乘凉。
> 
> 感谢原创

# 前言

简单说一下如何在CentOS7上搭建Jenkins环境

# 快速开始

### 1.安装基本命令

rz rs 是Linux上常用的上传下载命令，笔者从阿里去上买ECS云服务器是裸机，所以要安装这个命令，当然用Xftp也很方便
```
yum install lrzsz
```
具体版本快捷安装方式：

centos: yum -y install curl

ubuntu: sudo apt-get install curl

其他发行版，建议通过资源地址，下载Linux版本解压tar -xzvf xxx.tar.gz，然后安装。

### 2.安装JDK
##### 2.1 检查是否已安装
首先输入
```
java -version
```
查看CentOS上是否已有JDK
系统返回如下内容:
```
-bash: /usr/bin/java: No such file or directory 
```
表示没有JDK，可以继续。

##### 2.2 下载JDK安装包

首先到甲骨文官网下载jdk,官网地址如下:需下载(.gz)后缀的安装包
http://www.oracle.com
  
##### 2.3 安装jdk
 
###### 第一步:通过终端在/usr/local目录下新建java文件夹,命令行:
```
mkdir /usr/java 
```
###### 第二步:用rz上传下载好安装包到此目录下
```
rz
```
###### 第三步:解压压缩包,命令行:
 ```
 tar xvf JDKXXXXXXX.gz 
 ```
###### 第四步：设置JDK环境变量
 ```
 vi /etc/profile 
 ```
输入i,进入编辑模式
 ```
i
 ```
在里面添加如下内容:

 ```
JAVA_HOME=/usr/java/jdk1.8.0_20
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.: $JAVA_HOME/lib/dt.jar: $JAVA_HOME/lib/tools.jar
export JAVA_HOME
export PATH
export CLASSPATH
 ```
点击一下键盘上的"ESC",然后输入":wq"这样就将文本保存了
 ```
wq
 ```
进入 /usr/bin/目录
 ```
cd /usr/bin 
 ```
执行以下命令:
 ```
ln -s -f /usr/java/jdkXXXXXX/jre/bin/java 
ln -s -f /usr/java/jdkXXXXXX/bin/java 
 ```
执行以下命令
 ```
source /etc/profile  
 ```
###### 第五步：检验是否安装成功
 ```
java -version
 ```
返回显示：java version "1.XXXXX"
安装完成


1.git安装

(1)下载git命令:wget https://www.kernel.org/pub/software/scm/git/git-2.8.3.tar.gz          //选择一个目录后执行，我的是/usr/local/git

(2)解压git的tar包命令: tar -xzvf git-2.8.3.tar.gz  ，进入解压后的文件夹:cd git-2.8.3

(3)安装git安装编译所需要的依赖命令:yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker

(4)安装编译源码所需依赖的时候，yum自动帮你安装了git，这时候你需要先卸载这个旧版的git，命令：yum remove git

(5)编译git源码命令:make prefix=/usr/local/git all

(6)安装git至/usr/local/git路径命令：make prefix=/usr/local/git install

(5)配置git环境变量:vim /etc/profile  #编辑profile文件---->最下边添加git的路径即可：export PATH=$PATH:/usr/local/git/bin

(6)让profile文件修改生效命令:source /etc/profile

(7)检验git是否安装成功命令:git --version 

https://blog.csdn.net/qq_36631900/article/details/86504508

