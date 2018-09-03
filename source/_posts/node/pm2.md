---
layout: _posts
title: pm2
date: 2018-07-05 15:50:52
tags: pm2
categories:
- Node.js
description: pm2是一个带有负载均衡功能的应用进程管理器
---
### [官网](https://pm2.io/doc/en/runtime/overview/?utm_source=pm2&utm_medium=website&utm_campaign=rebranding)
### 安装
```
$ npm i -g pm2

// 服务器 alias
$ vi ~/.bash_profile
alias pm2='$HOME/node-v8.5.0-linux-x64/bin/pm2'
```
### 命令
#### 启动
```
$ pm2 start app.js

// 进程名 node，默认 app
$ pm2 start app.js --name node

// 根据CPU核数启动进程个数
$ pm2 start app.js -i 0

// 实时监控app.js的方式启动，当app.js文件有变动时，pm2会自动reload
$ pm2 start app.js --watch
```
#### 查看进程
```
$ pm2 list

// 查看进程详细信息，0为PM2进程id 
$ pm2 show 0
$ pm2 info 0
```
#### 监控
```
$ pm2 monit
```
#### 停止
```
$ pm2 stop all
$ pm2 stop name/进程
```
#### 重载
```
$ pm2 reload all
$ pm2 reload name/进程
```
#### 重启
```
$ pm2 restart all
$ pm2 restart name/进程
```
#### 删除
```
$ pm2 delete all
$ pm2 delete name/进程
```
#### 日志操作
```
$ pm2 logs app

// Empty all log file
$ pm2 flush

// Reload all logs
$ pm2 reloadLogs
```
#### 目录结构
```
$ ~/.pm2                   #will contain all PM2 related files
$ ~/.pm2/logs              #will contain all applications logs
$ ~/.pm2/pids              #will contain all applications pids
$ ~/.pm2/pm2.log           #PM2 logs
$ ~/.pm2/pm2.pid           #PM2 pid
$ ~/.pm2/rpc.sock          #Socket file for remote commands
$ ~/.pm2/pub.sock          #Socket file for publishable events
$ ~/.pm2/conf.js           #PM2 Configuration
```
### 自定义启动文件
```
{
  "apps":
    {
      "name": "test",
      "cwd": "/data/wwwroot/nodejs",
      "script": "./test.sh",
      "exec_interpreter": "bash",
      "min_uptime": "60s",
      "max_restarts": 30,
      "exec_mode" : "cluster_mode",
      "error_file" : "./test-err.log",
      "out_file": "./test-out.log",
      "pid_file": "./test.pid"
      "watch": false
    }
}

apps：json结构，apps是一个数组，每一个数组成员就是对应一个pm2中运行的应用
name：应用程序的名称
cwd：应用程序所在的目录
script：应用程序的脚本路径
exec_interpreter：应用程序的脚本类型，这里使用的shell，默认是nodejs
min_uptime：最小运行时间，这里设置的是60s即如果应用程序在60s内退出，pm2会认为程序异常退出，此时触发重启max_restarts设置数量
max_restarts：设置应用程序异常退出重启的次数，默认15次（从0开始计数）
exec_mode：应用程序启动模式，这里设置的是cluster_mode（集群），默认是fork
error_file：自定义应用程序的错误日志文件
out_file：自定义应用程序日志文件
pid_file：自定义应用程序的pid文件
watch：是否启用监控模式，默认是false。如果设置成true，当应用程序变动时，pm2会自动重载。这里也可以设置你要监控的文件。
```