---
title: Flask安装与相关知识
date: 2018-06-20 15:10:26
tags: 
- python
categories: 
- 后端开发
description: 
- 学习 flask 的过程及基础知识点.
---

## 一、[什么是flask?]()
Flask是一个Python编写的Web 微框架，让我们可以使用Python语言快速实现一个网站或Web服务
- [官网英文文档](http://flask.pocoo.org/docs/1.0/)
- [中文文档](http://dormousehole.readthedocs.io/en/latest/)
## 二、安装
### 虚拟环境
##### `python3`
```
$ mkdir myproject
$ cd muproject
$ python3 -m venv venv
```
##### `python2`
```
$ virtualenv venv
```
##### 激活虚拟环境
```
$ . venv/bin/activate
```
##### 退出虚拟环境
```
$ deactivate
```
### 安装 `Flask`
```
$ pip install Flask
```
## 三、[开发]()
- [教程地址](http://dormousehole.readthedocs.io/en/latest/tutorial/index.html#tutorial)

### `__init__.py`
`__init__.py` 有两个作用：一是包含应用工厂；二是 告诉 Python flaskr 文件夹应当视作为一个包。
### [`python3 os 模块`](http://www.pythondoc.com/pythontutorial3/stdlib.html#tut-os-interface)
`os` 模块提供了非常丰富的方法用来处理文件和目录
## 四、[运行]()
### Linux and Mac
```
$ export FLASK_APP=__init__
$ export FLASK_ENV=development
$ flask run
```
### Windows
```
$ set FLASK_APP=__init__
$ set FLASK_ENV=development
$ flask run
```