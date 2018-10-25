---
layout: _posts
title: node.js及npm的一些事
date: 2017-02-22 11:48:55
tags: csdn
categories: code
description: node.js及npm的一些事
---

------Windows---------
gulp是基于nodejs，理所当然需要安装nodejs
说明：什么是命令行？命令行在OSX是终端（Terminal），在windows是命令提示符（Command Prompt）
简单介绍gulp在使用过程中常用命令，打开命令提示符执行下列命令（打开方式：window + r 输入cmd回车）：
cd定位到目录，用法：cd + 路径 ；
dir列出文件列表；
cls清空命令提示符窗口内容。

node -v查看安装的nodejs版本，出现版本号，说明刚刚已正确安装nodejs。PS：未能出现版本号，请尝试注销电脑重试；

npm（node package manager）nodejs的包管理器，用于node插件管理（包括安装、卸载、管理依赖等）；
npm -v查看npm的版本号，npm是在安装nodejs时一同安装的nodejs包管理器，那它有什么用呢？稍后解释；
使用npm安装插件：命令提示符执行npm install <name> [-g] [--save-dev]；
<name>：node插件名称。例：npm install gulp-less --save-dev
-g：全局安装。将会安装在C:\Users\Administrator\AppData\Roaming\npm，并且写入系统环境变量；
    非全局安装：将会安装在当前定位目录；
    全局安装可以通过命令行在任何地方调用它，本地安装将安装在定位目录的node_modules文件夹下，通过require()调用；
--save：将保存配置信息至package.json（package.json是nodejs项目配置文件）；
-dev：保存至package.json的devDependencies节点，不指定-dev将保存至dependencies节点；
      一般保存在dependencies的像这些express/ejs/body-parser等等
为什么要保存至package.json？
  因为node插件包相对来说非常庞大，所以不加入版本管理，将配置信息写入package.json并将其加入版本管理，
  其他开发者对应下载即可（命令提示符执行npm install，则会根据package.json下载所有需要的包，
  npm install --production只下载dependencies节点的包）。

选装cnpm
说明：因为npm安装插件是从国外服务器下载，受网络影响大，可能出现异常，如果npm的服务器在中国就好了，
所以我们乐于分享的淘宝团队干了这事。32个！来自官网：“这是一个完整 npmjs.org 镜像，
你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。”；

手动新建package.json文件
{
  "name": "test",   //项目名称（必须）
  "version": "1.0.0",   //项目版本（必须）
  "description": "This is for study gulp project !",   //项目描述（必须）
  "homepage": "",   //项目主页
  "repository": {    //项目资源库
    "type": "git",
    "url": "https://git.oschina.net/xxxx"
  },
  "author": {    //项目作者信息
    "name": "surging",
    "email": "surging2@qq.com"
  },
  "license": "ISC",    //项目许可协议
  "devDependencies": {    //项目依赖的插件
    "gulp": "^3.8.11",
    "gulp-less": "^3.0.0"
  }
}
或者
npm init   //创建package.json文件

npm install   //依赖package.json文件安装插件（依赖）

npm -g (gulp/webpack/...)   //全局安装

npm (gulp/webpack/...)   //目录内安装

npm (gulp/webpack/...) --save-dev   //目录内安装并写入package.json文件，更新 devDependencies 值
//至于为什么在全局安装gulp后，还需要在项目中本地安装一次，
//有兴趣的可以看下stackoverflow上有人做出的回答：
//why-do-we-need-to-install-gulp-globally-and-locally、what-is-the-point-of-double-install-in-gulp。
//大体就是为了版本的灵活性，但如果没理解那也不必太去纠结这个问题，只需要知道通常我们是要这样做就行了。





-------MAC OSX----------
which node/npm    查找node/npm路径

sudo chown -R $USER /usr/local      获取本地管理员权限，接下来会输入密码

fetchMetadata   npm install 安装时获取资源中，由于网络原因，会卡在这里
解决办法，安装淘宝镜像：npm install -g cnpm --registry=https://registry.npm.taobao.org
然后cnpm







-报错-------------------------------------------------

Refusing to install (gulp/webpack/...) as a dependency of itself
npm init新建package.json文件时name(项目名称)为(gulp/webpack/...),重复了
