---
layout: _posts
title: webpack
date: 2017-02-22 19:31:03
tags: csdn
categories: code
description: webpack
---

$ npm install webpack -g    //我们通过npm来将webpack安装到全局

$ npm init // 用于初始化项目的package.json

一个最简单的webpack
// webpack.config.js
var path = require("path");
module.exports = {
	entry: './src/entry.js', //演示单入口文件
	output: {
		path: path.join(__dirname, 'out'),  //打包输出的路径
		filename: 'bundle.js',			  //打包后的名字
		publicPath: "./out/"				//html引用路径，在这里是本地地址。
	}
};

//准备好后，我们就要启动webpack了
// webpack 命令行的几种基本命令
$ webpack // 最基本的启动webpack方法
$ webpack -w // 提供watch方法，实时进行打包更新
$ webpack -p // 对打包后的文件进行压缩，提供production
$ webpack -d // 提供source map，方便调试。

//loader加载器
$ npm install style-loader css-loader url-loader babel-core babel-loader sass-loader node-sass file-loader --save-dev

//模块单独打包
var ExtractTextPlugin = require('extract-text-webpack-plugin');