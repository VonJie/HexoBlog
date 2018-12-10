---
layout: _posts
title: Lunix 系统命令
date: 2018-03-20 14:27:10
tags: Lunix
categories: 技术
---

### `tar` 压缩打包、解压文件
```
# 将 file 目录压缩打包成 name.tar
$ tar cvfz name.tar file

# 解压 name.tar
$ tar xvfz name.tar

# 解压 name.tar 到 file
$ tar xvfz name.tar -C file
```
<!-- more -->
### `scp` secure copy 远程拷贝
```
# 本质是利用SSH传输文件
# scp [参数] [原路径] [目标路径]

# 将本地文件 name.tar 拷贝到服务器 path 目录下
$ scp name.tar node@120.92.103.226:path

# 将服务器 path 目录下的 name.tar 拷贝到 本地 file 下
$ scp node@120.92.103.226:path/name.tar file
```

### `Centos` 系统相关
```
$ cat /etc/redhat-release 

// 查看 lunix 系统信息
$ uname -a
$ cat /proc/version

// 语言环境
locale
```

### 删除文件
```
// 单一或少量文件
$ rm -rf *.*

// 大量文件
$ ls | args rm -rf

// 删除一天之前的文件
find . -type f -mtime +1 -exec rm {} \;
```

### 内存分配
```
$ vi /etc/security/limits.d/20-nproc.conf
```

### 查看 端口/进程
```
$ lsof -i tcp:8181

$ netstat -anp | grep 8181

// 查看 node 相关进程
$ ps -ef | grep node

// 杀掉进程
$ kill 17898

// 杀掉所有相关进程
$ ps -efww|grep chromium |grep -v grep|cut -c 9-15|xargs kill -9
```

### `crontab` 定时器
```
// 列出
crontab -l

// 编辑
crontab -e

## get the new proxylist.json every 2 hours
1,11,21,31,41,51 * * * *  curl -sS 'http://proxy.badapp.cn:7777/listallproxy?token=thisismytoken&ptype=fast' -o ~/spiderdata/proxylist.json --compressed

## test
## * * * * * echo 'test' >> ~/test.log

## proc restart node every 1 hours
0 * * * * sh ~/restart.sh

## clear headless chrome every 1 hours
0 * * * * ps -efww|grep chromium |grep -v grep|cut -c 9-15|xargs kill -9
ps -fu node | grep chromium | grep -v grep | awk '{print $2}' | xargs kill -9

## clear cheese.log every 1 hours
0 * * * * rm -rf ~/wxjg-js-server/logs/*.*

## clear html every 1 hours
0 * * * * rm -rf /filestore/node/htmls/*.*
```

### `ls`
```
ls -l

// 按时间排序
ls -l -tr

// 文件数量
ls -l | grep -c '^-'
ps -fu node | grep chromium | wc -l
```