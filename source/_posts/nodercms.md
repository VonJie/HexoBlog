---
title: NoderCMS 相关
tags: 
- Node_CMS
categories: 
- Node.js
description: 
- 基于 Node.js & MongoDB 的 轻量级 CMS 内容管理系统
---

## 一. [NoderCms](http://www.nodercms.com/)

> [下载地址](http://www.nodercms.com/download)
> [github 仓库地址](https://github.com/welkinwong/nodercms)
> [演示地址](http://demo.nodercms.com)
> [演示后台](http://demo.nodercms.com/admin)
> 后台账号密码：ghost@nodercms.com 123456

## 二. 本地运行
- ##### 安装包

```
$ npm install --production
```

- ##### 启动

``` 
$ npm start
```

## 三. 连接至服务器 Mongodb
- ##### [下载 openvpn 客户端](http://xclient.info/s/viscosity.html?t=033774a17acee6ea5c77b73f98f6d278de2bb635)
- ##### 服务器配置 openvpn 相关，并且给我一个 openvpn 相关配置的文件夹
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-110955.png)
- 相关配置参数，导入文件夹自动生成的
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/1528421985822.jpg)
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-094018.png)
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-094031.png)
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-094043.png)
- 输入服务端给你设置的账号密码
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-105256.png)


## 四. MongoDB 查看
- ##### [下载 nosqlbooster](https://nosqlbooster.com/downloads) 
- ##### 连接至服务器 10.254.242.36
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-105530.png)
- ##### 新建数据库表
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-105648.png)
- ##### 命名
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-105743.png)
- ##### 添加用户
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-105812.png)
- ##### 设置账号密码，读写权限，run 执行命令
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/nodercms/WX20180608-110236.png)
- 删除数据库表，鼠标右键 Drop Database

## 五. 网页配置
- 打开本地网页 http://localhost:3000/admin/install
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/WX20180608-111348.png)
- MongoDB 数据库连接配置
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/WX20180608-111430.png)
- 设置 CMS 相关参数
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/WX20180608-111755.png)
- 登陆系统
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/WX20180608-111822.png)
- 进入后台主页
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/nodercms/WX20180608-111837.png)

## 六. 注意事项
- 重新安装
  - 删除数据库，删除 install.lock
  - 再次 npm start 后，在连接数据库环节回一直报错
  - 删除本地文件，重走 流程一 即可。