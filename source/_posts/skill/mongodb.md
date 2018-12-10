---
layout: _posts
title: mongodb
date: 2018-06-26 12:03:08
tags: mongodb
categories: 技术
description: 数据库 mongodb 相关操作 

---
#### [官网文档](http://www.mongoing.com/docs/)
#### 创建数据库
```
use `graphql`
```
#### 删除数据库
```
db.dropDatabase()
```
<!-- more -->
#### 创建集合
```
db.createColletion('Cat')
```
#### 查看集合
```
db.Cats.find()
```
#### 删除集合
```
db.Cats.drop()
```
#### 插入文档
```
db.Cats.insertOne({
    name: 'juday'
})
```
#### 更新文档
```
db.Cats.updateOne({name: 'juday'},{$set:{name: 'judy'}})
```
#### 删除文档
```
db.Cats.remove({name: 'judy'})
```
#### 删除文档字段
```
db.yourcollection.update({},{$unset:{"field":""}},false,true)
```
