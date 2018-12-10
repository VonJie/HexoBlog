---
layout: _posts
title: CURL 命令
date: 2018-06-20 23:34:50
tags: CURL
categories: 技术
---

#### [相关链接](http://man.linuxde.net/curl)
#### 语法
```
$ curl [option] [url]
```
#### `-i`
```
// 带上 protocol 信息
$ curl -i "http://www.mutool.cn"
```
#### `GET`
```
$ curl -i URL
```
#### `POST`
```
$ curl -d "id=1" URL
```
<!-- more -->
#### `PUT`
```
$ curl -T uploadfile URL

$ curl -i -H "Content-Type: application/json" -X POST -d '{"title":"Read a book"}' URL
```
#### `user-agent` 用户代理
```
$ curl -A "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" URL
```
#### cookie
```
$ curl -b "id=1" URL
```
#### header
```
$ curl -H "Content-Type: application/json" URL
```
#### 登录账号密码
```
$ curl -u name:password URL
```
#### 挂代理
```
$ curl -x 123.178.2.103:80001 URL
```
#### 文件下载
```
// 保存到当前文件夹
$ curl -O http:www.mutool.cn/index.html

// 另存为
$ curl -o mutool.html http:www.mutool.cn/index.html

// 循环下载
$ curl -O http://www.linux.com/dodo[1-5].JPG

// 下载重命名
$ curl -o #1_#2.JPG http://www.linux.com/{hello,bb}/dodo[1-5].JPG
// hello_dodo[1-5].JPG
```
#### 进度条显示当前的传送状态
```
$ curl -# URL
```
#### 伪造 `referer`
```
$ curl -e "www.linux.com" URL
```
