---
layout: _post
title: supervisor
date: 2018-06-22 14:58:27
tags: supervisor
categories: 
- 操作系统
description: 在 web 应用部署到线上后，需要保证应用一直处于运行状态，在遇到程序异常、报错等情况，导致 web 应用终止时，需要保证程序可以立刻重启，继续提供服务。所以，就需要一个工具，时刻监控 web 应用的运行情况，管理该进程。Supervisor 就是解决这种需求的工具，可以保证程序崩溃后，重新把程序启动起来等功能。
---

### 关于
Supervisor 是用 Python 开发的一个 client/server 服务，是 Linux/Unix 系统下的一个进程管理工具。
所以运行需要 python 虚拟环境
#### [官网](http://supervisord.org/)
### 配置
; 后面类容为注释
#### `alias`
```
alias proc='supervisorctl -c ~/bin/supervisord.conf'
```
#### `/bin/supervisord.conf`
```
[unix_http_server]
;file=/var/run/supervisor.node.sock
chmod=0700
file=/home/node/bin/supervisor.node.sock

[inet_http_server]         ; inet (TCP) server disabled by default
port=127.0.0.1:7902         ; (ip_address:port specifier, *:port for all iface)
username=innmall            ; (default is no username (open server))
password=hhxxttxs           ; (default is no password (open server))

[supervisord]
logfile=~/logs/supervisor.log ; (main log file;default $CWD/supervisord.log)
logfile_maxbytes=50MB        ; (max main logfile bytes b4 rotation;default 50MB)
logfile_backups=10           ; (num of main logfile rotation backups;default 10)
loglevel=info                ; (log level;default info; others: debug,warn,trace)
pidfile=/home/node/bin/supervisor.node.pid      ; (supervisord pidfile;default supervisord.pid)
nodaemon=false               ; (start in foreground if true;default false)
minfds=1024                  ; (min. avail startup file descriptors;default 1024)
minprocs=200                 ; (min. avail process descriptors;default 200)
;umask=022                   ; (process file creation umask;default 022)
;user=chrism                 ; (default is current user, required if root)
;identifier=supervisor       ; (supervisord identifier, default is 'supervisor')
;directory=/tmp              ; (default is not to cd during start)
;nocleanup=true              ; (don't clean up tempfiles at start;default false)
;childlogdir=/tmp            ; ('AUTO' child log dir, default $TEMP)
;environment=KEY="value"     ; (key value pairs to add to environment)
;strip_ansi=false            ; (strip ansi escape codes in logs; def. false)

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
;serverurl=unix:///var/run/supervisor.node.sock ; use a unix:// URL  for a unix socket
serverurl=unix:///home/node/bin/supervisor.node.sock ; use a unix:// URL  for a unix socket

[include]
files = supervisord.conf.d/*.conf
```
#### `/bin//supervisord.conf.d/nodejs.conf`
```
[program:node]
command=/home/node/node-v8.5.0-linux-x64/bin/node app.js              ; the program (relative uses PATH, can take args)
process_name=%(program_name)s           ; process_name expr (default %(program_name)s)
;numprocs=5                              ; number of processes copies to start (def 1)
directory=/home/node/wxjg-js-server/   ; directory to cwd to before exec (def no cwd)
autostart=true                          ; 设置改程序是否虽supervisor的启动而启动 (default: true)
autorestart=true                        ; 程序停止之后是否需要重新将其启动 (default: unexpected)
startsecs=3                             ; 重新启动时，等待的时间(def. 1)
startretries=10                         ; 重启程序的次数(default 3)
stopwaitsecs=10                         ; max num secs to wait b4 SIGKILL (default 10)
redirect_stderr=true                    ; redirect proc stderr to stdout (default false)
stdout_logfile=/home/node/logs/puppeteer.out
stopasgroup=true
user=node
umask=022

;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
;stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stderr_logfile=$HOME/log/webmainmovie.err
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=MODE="PRODUCTION"  ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)
;umask=022                     ; umask for process (default None)
;priority=999                  ; the relative start priority (default 999)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;exitcodes=0,2                 ; 'expected' exit codes for process (default 0,2)
;stopsignal=QUIT               ; signal used to kill process (default TERM)
```
#### `start.sh`
```
#!/bin/sh
. ~/.bash_profile

supervisord -c $HOME/bin/supervisord.conf
```
#### `update.sh`
```
#!/bin/sh
. ~/.bash_profile

cd ~/bin
supervisorctl -c supervisord.conf update
```