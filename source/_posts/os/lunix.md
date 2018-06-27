---
layout: _posts
title: Lunix 系统命令
date: 2018-06-22 14:27:10
tags: lunix
categories: 
- 操作系统
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

### `scp` secure copy 远程拷贝
```
# 本质是利用SSH传输文件
# scp [参数] [原路径] [目标路径]

# 将本地文件 name.tar 拷贝到服务器 path 目录下
$ scp name.tar node@120.92.103.226:path

# 将服务器 path 目录下的 name.tar 拷贝到 本地 file 下
$ scp node@120.92.103.226:path/name.tar file
```

### `Centos` 查看版本信息
```
$ cat /etc/redhat-release 

// 查看 lunix 系统信息
$ uname -a
$ cat /proc/version
```