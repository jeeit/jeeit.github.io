---
layout:     post
title:      「ElasticSearch」 之 部署篇
subtitle:   「ElasticSearch」 之 部署篇
date:       2019-01-18
author:     JEEIT
header-img: img/post-bg-mma-5.jpeg
catalog: true
tags:
    - ElasticSearch
    - JEEIT
---

> 正所谓前人栽树，后人乘凉。
> 
> 感谢 自己（原创）

# 前言

ElasticSearch 简称 ES 是一个基于Lucene的搜索服务器

ElasticSearch 的下载地址：

https://www.elastic.co/downloads/elasticsearch；

# 快速开始

### 1.Windows部署ES

下载好之后将其解压到你想要安装的目录：比如我的 D:\chengxu\ElasticSearch\elasticsearch-6.3.0 。

以上，就算安装好了。那么如何知道安装“好”了呢，运行一下就知道了。

进入到D:\chengxu\ElasticSearch\elasticsearch-6.3.0 \bin中，双击执行 elasticsearch.bat 。等待打印信息输出完之后打开浏览器，输入：localhost:9200 。页面显示如下，则说明安装好了。
 

### 2.CentOS7部署ES

###### 第一步:部署Nginx
网上有很多教程，就不一一说了
比如：https://www.linuxidc.com/Linux/2018-07/153183.htm
```
 卸载nginx
 1） [root@iZwz9iiexwyqe81pm0weshZ /]# yum remove nginx
 
 2） [root@iZwz9iiexwyqe81pm0weshZ /]# which nginx
```


### 3.开始部署JEE-UI的代码

JEEIT-UI：
    开源代码地址：https://gitee.com/jee_it/jeeit-cloud-ui
    打包后的代码地址：https://gitee.com/jee_it/jeeit-cloud-ui-dist        
 
###### 第一步:通过终端在/目录下新建jeeit文件夹,命令行:
```
mkdir /jeeit/jeeit-cloud-ui-dist
```
###### 第二步:用rz上传打包好的jeeit-ui压缩包到此目录下
```
rz
```
###### 第三步:解压压缩包,命令行:
 ```
unzip jeeit-cloud-ui-dist.zip 
 ```
 










