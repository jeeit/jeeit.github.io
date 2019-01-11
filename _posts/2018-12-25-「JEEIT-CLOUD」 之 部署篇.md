---
layout:     post
title:      「JEEIT-CLOUD」 之 部署篇
subtitle:   「JEEIT-CLOUD」 之 部署篇
date:       2019-1-10
author:     JEEIT
header-img: img/post-bg-mma-5.jpeg
catalog: true
tags:
    - Linux
    - JEEIT
---

> 正所谓前人栽树，后人乘凉。
> 
> 感谢 自己（原创）

# 前言

简单叙述一下如何在CentOS7上部署JEEIT-CLOUD运行环境

# 快速开始

### 1.CentOS7部署MYSQL

##### 第一步:卸载已安装的mysql
查看是否有安装的mysql
```
rpm -qa | grep -i mysql // 查看命令1

yum list install mysql* // 查看命令2
```
卸载mysql安装包

```
yum remove mysql mysql-server mysql-libs compat-mysql51
yum remove mysql-community-release
rpm -e --nodeps mysql-community-libs-5.7.22-1.el7.x86_64
rpm -e –nodeps mysql57-community-release-el7-11.noarch
```

总之删到通过上面两种命令查不出来任何有关mysql的东西。

删除残留的mysql目录或文件：
查询mysql安装目录

```
whereis mysql 

find / -name mysql
```
删除查询出的目录，我的这个是在没有删除安装包的情况下截的图，所以可能比较多。


```
rm -rf /usr/lib64/mysql
rm -rf /usr/share/mysql
rm -rf /usr/bin/mysql
rm -rf /etc/logrotate.d/mysql
rm -rf /var/lib/mysql
rm -rf /var/lib/mysql/mysql

```
总之删到通过上面两种命令查不出来任何有关mysql的东西。

继续删除
删除mysql 配置文件
```
rm –rf /usr/my.cnf
rm -rf /root/.mysql_sercret  
```
删除mysql开机自启动服务

```
chkconfig --list | grep -i mysql
chkconfig --del mysqld  // 服务名为你设置时候自己设置的名字
```
至此就卸载干净了


##### 第二步:安装mysql
下载并安装mysql的YUM源：
选择一个目录下载并安装


```
mkdir soft
cd soft

wget http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm // 下载mysql yum源
rpm -ivh mysql57-community-release-el7-11.noarch.rpm // 安装yum源
```

yum源安装成功 

接下在就是正式安装mysql了
```
yum install mysql-community-server
```
安装mysql成功 

启动mysql（没报错直接去>登录）
```
service mysqld start
```

如果出现以下错误：

```
ERROR 1045 (28000): Access denied for user ‘root’@’localhost’ (using password: NO)
```
首先停止mysql服务


```
service mysqld stop
```
再以不检查权限的方式启动
```
mysqld --skip-grant-tables &
```
又出现以下错误：
```
[ERROR] Fatal error: Please read “Security” section of the manual to find out how to run mysqld as root!
```
执行命令以root权限启动
```
mysqld --user=root --skip-grant-tables &
```
刚安装之后的mysql生成了一个初始密码，通过以下命令获得初始密码
```
grep 'temporary password' /var/log/mysqld.log
```
登录 mysql
```
mysql -uroot -p
```
 
用该初始密码登录后，必须马上修改新的密码，不然会报如下错误：
 
```
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before
```
如果你想要设置一个简单的测试密码的话，比如设置为123456，会提示这个错误，报错的意思就是你的密码不符合要求
  
```
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```
这个其实与validate_password_policy的值有关。
默认是1，即MEDIUM，所以刚开始设置的密码必须符合长度，且必须含有数字，小写或大写字母，特殊字符。 
有时候，只是为了自己测试，不想密码设置得那么复杂，譬如说，我只想设置root的密码为123456。 
必须修改两个全局参数：

首先，修改validate_password_policy参数的值
```
mysql> set global validate_password_policy=0;
Query OK, 0 rows affected (0.00 sec)
```
validate_password_length(密码长度)参数默认为8，我们修改为1

 ```
 mysql> set global validate_password_length=1;
 Query OK, 0 rows affected (0.00 sec)
 ```
完成之后再次执行修改密码语句即可成功
 ```
 UPDATE mysql.user SET authentication_string=PASSWORD('密码') where USER='root';
 ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';
 SET PASSWORD FOR root=PASSWORD('密码');
 
 flush privileges; // 刷新设置立即生效
 
 exit  // 退出,或者使用 quit 命令
 ```
再次进入

```
mysql -uroot –p // 会提示输入密码
```
输入密码，成功则密码设置完成了
设置root权限的远程访问
```
grant all privileges on *.* to root@"%" identified by "密码";

flush privileges; // 刷新设置立即生效
```
然后就可以通过navicat(或者其他工具)远程连接了


