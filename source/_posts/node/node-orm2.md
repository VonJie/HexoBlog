---
layout: _post
title: node-orm 相关
date: 2018-08-26 10:19:54
tags: node-orm
categories:
- Node.js
description: Mongoose 是在 node.js 环境下对 mongodb 进行便捷操作的对象模型工具
---

## 一、关于 `node-orm2`
> ORM2是一款基于Node.js实现的ORM框架。
ORM指对象关系映射（英语：(Object Relational Mapping，简称ORM，或O/RM，或O/R mapping），是一种程序技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。简单的说，ORM是通过使用描述对象和数据库之间映射的元数据，将程序中的对象自动持久化到关系数据库中。

[node-orm2 github 仓库](https://github.com/dresende/node-orm2)
[node-orm2 教程](https://www.cnblogs.com/showtime813/p/4552086.html)
[node-orm2 相关问题](https://www.cnblogs.com/fengxiang/p/3422327.html)
## 二 使用方法
### 数据库的连接
```
export async function connectSql () {
	return new Promise((resolve, reject) => {
		// 'mysql://sandbox:sandbox@2018@127.0.0.1:3306/test_sandbox'
		orm.connectAsync($conf, function(err, db){
		  if (err) throw err;

		  console.log('MySql is connecting');

      global.db = db;

		  resolve(db);
		})
	})
}

export const disconnectSql = () => {
	// 数据库断开连接
  global.db.close();
  console.log('MySql connect off')
}
```
### 数据库的方法
```
const getPubCataDef = (args, type, ctx) => {
  authorize(ctx);

  return new Promise(resolve => {
    return (async () => {

      let db = await connectSql();

      // define 方法的第一个参数为表名
      db.define('pub_cata_def', {
        id: Number,
        groupid: String
      });

      let PubCataDef = db.models.pub_cata_def;

      switch (type){
        // 查
        case 'read':
          PubCataDef.find(args, function(err, cata){
            if(err) throw err;
            // 更新数据
            resolve(cata);
            disconnectSql();
            return ;
          })
        break;
        // 增
        case 'create':
          Users.create([args], function(err, user){
            if(err) throw err;
            resolve(user);
            disconnectSql();
            return;
          })
        break;
        // 更
        case 'update':
          Users.find({ id: args.id }).each(function (user) {
            for(let key in args){
              user[key] = args[key]
            }
            resolve(user);
          }).save(function (err) {
            if(err) throw err;

            disconnectSql();
            return;
          });
        break;
        // 删
        case 'delete':
          Users.find(args).remove(function (err, user) {
            if(err){
              resolve(err);
              throw err;
            }
            resolve('success');
            disconnectSql();
            return;
          });
        break;
      }
    })()
  })
}
```
####  `Model` 定义
官方的Model加载定义是在连接数据库的时候，如下:
```
var orm = require("orm");

orm.connect("mysql://username:password@host/database", function (err, db) {
  if (err) throw err;

	var Person = db.define("person", {
		name      : String,
		surname   : String,
		age       : Number, // FLOAT
		male      : Boolean,
		continent : [ "Europe", "America", "Asia", "Africa", "Australia", "Antarctica" ], // ENUM type
		photo     : Buffer, // BLOB/BINARY
		data      : Object // JSON encoded
	}, {
		methods: {
			fullName: function () {
				return this.name + ' ' + this.surname;
			}
		},
		validations: {
			age: orm.enforce.ranges.number(18, undefined, "under-age")
		}
	});

    // add the table to the database
	db.sync(function(err) {
		if (err) throw err;

		// add a row to the person table
		Person.create({ id: 1, name: "John", surname: "Doe", age: 27 }, function(err) {
			if (err) throw err;

				// query the person table by surname
				Person.find({ surname: "Doe" }, function (err, people) {
			        // SQL: "SELECT * FROM person WHERE surname = 'Doe'"
		        	if (err) throw err;

			        console.log("People found: %d", people.length);
			        console.log("First person: %s, age %d", people[0].fullName(), people[0].age);

			        people[0].age = 16;
			        people[0].save(function (err) {
			            // err.msg = "under-age";
		        });
		    });

		});
	});
});
```
可是这样太冗余，对于大项目表又多的情况明显不合适，所以它还有一种加载定义Model方式，即Model作为单独的一个文件。
```
//定义表结构Model
module.exports = function (db, cb) {
    //define方法的第一个参数为表名
    db.define('xxx', {
        innerid: { type: 'serial', key: true } , //主键
        name: String,
        identifying: String,
        purpose_code: Number,
        org_id: String,
        position_x: Number,
        position_y: Number,
        isenabled: Number,
        isdeleted: Number
    });
    return cb();
}
```
加载
```
//db为数据库连接,相当于connection;  load第一个参数为Model的路径
 db.load('XXX', function (err) {
  //加载model错误，记录异常
  if (err) {
    return callback(err,xxx);
  }
  callback(null,xxx);
})
```
#### 数据插入 `create`
```
Person.create([
    {
        name: "John",
        surname: "Doe",
        age: 25,
        male: true
    },
    {
        name: "Liza",
        surname: "Kollan",
        age: 19,
        male: false
    }
], function (err, items) {
    // err - description of the error or null
    // items - array of inserted items
});
```
#### 数据查找 `read`
##### 1.Model.get(第一个参数为id的值):
```
Person.get(4, function(err, person) {
    console.log( person.fullName() );
})
```
##### 2.Model.find([ conditions ] [, options ] [, limit ] [, order ] [, cb ])
```
Person.find({ name: "John", surname: "Doe" }, 3, function (err, people) {
    // finds people with name='John' AND surname='Doe' and returns the first 3
});
```
##### 3.Model.find链式写法
```
Person.find({ name: "John", surname: "Doe" }, 3, function (err, people) {
    // finds people with name='John' AND surname='Doe' and returns the first 3
});
```
#### 数据删除 `delete`
```
Person.find({ surname: "Doe" }).remove(function (err) {
    // Does gone..
});
```
#### 数据更新 `update`
```
Person.find({ surname: "Doe" }).each(function (person) {
    person.surname = "Dean";
}).save(function (err) {
    // done!
});
```
#### 取COUNT
```
Person.find({ surname: "Doe" }).count(function (err, people) {
    // people = number of people with surname="Doe"
});
```
#### 判断是否存在
```
Person.exists({ surname: "Doe" }, function (err, exists) {
    console.log("We %s Does in our db", exists ? "have" : "don't have");
});
```
#### 多表查询替代方法 && 直接执行SQL
```
db.driver.execQuery("SELECT id, email FROM user", function (err, data) { ... })

// 上面是直接执行SQL，下面是类似参数化。   列用??表示, 列值用?表示。   建议使用下面这种
db.driver.execQuery(
  "SELECT user.??, user.?? FROM user WHERE user.?? LIKE ? AND user.?? > ?",
  ['id', 'name', 'name', 'john', 'id', 55],
  function (err, data) { ... }
)
```