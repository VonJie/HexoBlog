---
layout: _post
title: docker
date: 2018-06-25 09:37:01
tags: docker
categories: 
- 操作系统
---

#### 列出容器
```
$ docker ps

//-- 附加参数 --//
$ -a  // 显示所有容器
$ -f  // 根据条件筛选
$ -l  // 显示最近创建
```

#### MongoDB 相关操作
```
$ docker run --name nodercms -d mongo:3.6.5
$ docker exec -t nodercms bash
$ mongo
```
