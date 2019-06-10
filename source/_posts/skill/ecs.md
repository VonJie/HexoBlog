---
layout: _post
title: 阿里云ECS
date: 2019-05-30 14:50:49
tags: 云服务器
categories: 技术
---

## 配置 Nginx
### 1、准备工作
Nginx的安装依赖于以下三个包，意思就是在安装Nginx之前首先必须安装一下的三个包，注意安装顺序如下：
- 1 SSL功能需要openssl库，直接通过yum安装: #yum install openssl
- 2 gzip模块需要zlib库，直接通过yum安装: #yum install zlib
- 3 rewrite模块需要pcre库，直接通过yum安装: #yum install pcre

### 2、安装Nginx依赖项和Nginx
- 1 使用yum安装nginx需要包括Nginx的库，安装Nginx的库
```
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
- 2 使用下面命令安装nginx
```
yum install nginx
```
- 3 启动Nginx
```
#service nginx start
```
### 云服务器开启`80`端口