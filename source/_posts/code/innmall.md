---
layout: _posts
title: 鹰漠酒馆_代码笔记
date: 2017-02-26 19:31:03
tags: csdn
categories: code
description: 鹰漠酒馆_代码笔记
---

--MAC OS 启动--
打开多个终端
终端1:npm run mock 启动模拟api
终端2:npm run dev 启动页面
这样就可以跑起来了
或者iterm2 command+D 分出两个终端启动启动
-- windows 启动--。
直接 npm run dev 会失败，  报错export是无效的命令==>  package scripts里面import 改为 set
localhost:9090 和 0.0.0.0：8080 无效 
127.0.0.1:9090 和 127.0.0.1:8080 访问 
---Package---------------------------------------------------------------------------
$ npm install --save/--save-dev  //--save生产环境  --save-dev开发环境
devDependencies  里面的插件只用于开发环境，不用于生产环境，而 dependencies  是需要发布到生产环境的。

$ npm install json-server  //模拟API 

package.json中的script来配置脚本，NODE_ENV代表node进程，或者说时环境变量
下面的mock和test就是配置的变量，node运行时执行对应的脚本
$ npm run mock //启动项目（通过json-server模拟json数据，本地运行）
$ npm run test //用于打包，上线

Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。
// 转码前
input.map(item => item + 1);
// 转码后
input.map(function (item) {
  return item + 1;
});
babel配置
  {
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
  }
  
$ npm install classnames --save-dev
在React中编写模板时给标签添加class。如果是固定的className="XX"就可以了。
如果要根据状态值动态应用或去除，或使用多个class时就麻烦了。可以使用classnames模块来解决：
var classnames= require('classnames');
classnames('foo', 'bar'); // => 'foo bar' 
classNames('foo', { bar: true }); // => 'foo bar' 

$ npm install clean-webpack-plugin --save-dev
clean-webpack-plugin用于在building之前删除你以前build过的文件

$ npm install gulp-eslint –save-dev
EsLint帮助我们检查Javascript编程时的语法错误。比如：在Javascript应用中，你很难找到你漏泄的变量或者方法。
EsLint能够帮助我们分析JS代码，找到bug并确保一定程度的JS语法书写的正确性。

$ npm install whatwg-fetch --save
异步请求API
fetch(location.herf, {
    method: "get"
}).then(function(response) {
    return response.text()
}).then(function(body) {
    console.log(body)
})


---Webpack.config.js-----------------------------------------------------------------
$$ import path from 'path';
//NodeJS中的Path对象，用于处理目录的对象，提高开发效率。
1、格式化路径 path.normalize(p)
path.normalize('/foo/bar//baz/asdf/quux/..');
特点：将不符合规范的路径格式化，简化开发人员中处理各种复杂的路径判断
=> '/foo/bar/baz/asdf'
2、路径合并 path.join([path1], [path2], […])
特点：将所有名称用path.seq串联起来，然后用normailze格式化
path.join('///foo', 'bar', '//baz/asdf', 'quux', '..');
=>'/foo/bar/baz/asdf'
3、路径寻航 path.resolve([from …], to)
特点：相当于不断的调用系统的cd命令,调用cd是为创建文件夹
path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subfile')
//相当于：
cd foo/bar
cd /tmp/file/
cd ..
cd a/../subfile
pwd
4、相对路径 path.relative(from, to)
特点：返回某个路径下相对于另一个路径的相对位置串，相当于：path.resolve(from, path.relative(from, to)) == path.resolve(to)
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb')
=>'..\\..\\impl\\bbb'
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb')
=>'../../impl/bbb'
5、文件夹名称 path.dirname(p)
特点：返回路径的所在的文件夹名称
path.dirname('/foo/bar/baz/asdf/quux')
=>'/foo/bar/baz/asdf'
......更多请百度

$ npm install precss --save
var precss = require('precss');
Precss 可以在 CSS 文件中使用 Sass 类型的 Markup。

$ npm install autoprefixer --save
$$ import autoprefixer from 'autoprefixer';
Autoprefixer解析CSS文件并且添加浏览器前缀到CSS规则里，使用Can I Use的数据来决定哪些前缀是需要的
a{
     transition :transform 1s
}
Autoprefixer使用一个数据库根据当前浏览器的普及度以及属性支持提供给你前缀：
a{
     -webkit-transition :-webkit-transform 1s;
     transition :-ms-transform 1s;
     transition :transform 1s
}

$ npm install html-webpack-plugin --save
$$ import HtmlWebpackPlugin from 'html-webpack-plugin';
这个插件用来简化创建服务于 webpack bundle 的 HTML 文件，尤其是对于在文件名中包含了 hash 值，
而这个值在每次编译的时候都发生变化的情况，会生成html

$$ new webpack.optimize.CommonsChunkPlugin('common', 'bundle.js'),
CommonsChunkPlugin是提取公共代码块用的

$$ new webpack.optimize.UglifyJsPlugin({
$$     compress: {
$$      warnings: false,
$$    },
$$  })
UglifyJsPlugin是js代码压缩工具

$$ new webpack.optimize.OccurrenceOrderPlugin(),
根据模块调用次数，给模块分配ids，常被调用的ids分配更短的id，使得ids可预测，降低文件大小，该模块推荐使用
[ webpack 常用plugin和loader...https://segmentfault.com/a/1190000005106383 ]

$ npm whatwg-fetch --save
fetch(url).then(response => response.json())
  .then(data => console.log(data))
  .catch(e => console.log("Oops, error", e))

---ES6...-------------------------------------------------------------------------------
Arrow Function（箭头函数） =>
x => x * x
上面的箭头函数相当于：
function (x) {
    return x * x;
}
箭头函数相当于匿名函数，并且简化了函数定义。
箭头函数有两种格式，一种像上面的，只包含一个表达式，连{ ... }和return都省略掉了。
还有一种可以包含多条语句，这时候就不能省略{ ... }和return：
x => {
    if (x > 0) {
        return x * x;
    }
    else {
        return - x * x;
    }
}
箭头函数看上去是匿名函数的一种简写，但实际上，箭头函数和匿名函数有个明显的区别：
箭头函数内部的this是词法作用域，由上下文确定。
如果使用箭头函数，以前的那种hack写法：var that = this;
由于this在箭头函数中已经按照词法作用域绑定了，所以，
用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略：
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2015); // 25

FormData对象
通过 JavaScript 用一些键值对来模拟一系列表单控件
var oMyForm = new FormData();
oMyForm.append("accountnum", 123456); // 数字123456被立即转换成字符串"123456"

---app.jsx----------------------------------------------------------------------------
let body = new FormData();
利用FormData对象,你可以使用一系列的键值对来模拟一个完整的表单,然后使用XMLHttpRequest发送这个"表单".


---前端框架--------------------------------------------------------------------------
[浅谈移动前端的最佳事件...http://www.cnblogs.com/yexiaochai/p/4219523.html]
import ReactIScroll from 'react-iscroll'
iscroll 