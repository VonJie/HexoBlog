---
layout: _post
title: mongoose
date: 2018-06-26 10:19:54
tags: mongoose
categories:
- Node.js
description: Mongoose 是在 node.js 环境下对 mongodb 进行便捷操作的对象模型工具
---

## 什么是 `Mongoose`
### [官网](http://mongoosejs.com/)
### [中文文档 v4](https://mongoose.shujuwajue.com/)
`Mongoose` 是在 `node.js` 环境下对 `mongodb` 进行便捷操作的对象模型工具
---
## Api 文档
### `Connections`
#### `connect`
```
// es5
const mongoose = require('mongoose')
// es6
import mongoose from mongoose
// 连接本地
mongoose.connect(`mongodb://localhost/Mongodb`)

// 账号密码连接内网 IP
mongoose.connect(`mongodb://User:Password@10.0.0.211:27017/Mongodb`)
```
### 模式 `Schemas`
`Mongoose` 的一切都始于一个 `Schema` 。每个 `schema` 映射到 `MongoDB` 的集合 `collection` 和定义该集合 `collection` 中的文档的形式。
#### 定义
```
import { Schema } from mongoose

let blogSchema = new Schema({
    title: String,
    comments: [
        {
            body: String,
            date: Date
        }
    ],
    date: {
        type: Date,
        fefault: Date.now
    },
    hidden: Boolean,
    meta: {
        votes: Number
    },
    binary:  Buffer,
    mixed: Schema.Types.Mixed,
    _someId: Schema.Types.ObjectId,
})
```
#### 类型
##### `String`
##### `Number`
##### `Date`
- 内置的日期的方法
- 在mongoose用英语改变跟踪逻辑不是钩子

##### `Buffer`
##### `Boolean`
##### `Mixed`
- 一个“什么都行”的SchemaType，它的灵活性会带来权衡它难维护
- 以下是等价的
```
let Any = new Schema({ any: {} });
let Any = new Schema({ any: Schema.Types.Mixed });
```
- 保存修改
```
person.anything = { x: [3, 4, { y: "changed" }] };
person.markModified('anything');
person.save(); // anything will now get saved
```

##### `Objectid`
```
let Car = new Schema({ driver: Schema.Types.ObjectId });
```
##### `Array`

### 模型 `Model`
#### 创建
```
let uSchema = new Schema({
    name: String
})

let uModel = mongoose.model('uModel', uSchema)
```
#### 保存实例
```
let silly = new uModel({
    name: 'silly'
})

silly.save(function(err){
    if(err) return handle(err)
})
```

### 查询 `Query`
```
let uSchema = new Schema({...obj})

let uModel = mongoose.model('uModel', yourSchema);

uModel.find({name: /^s/i}, function(err, res){
    // res is your query result
})
```