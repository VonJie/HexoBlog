---
layout: _posts
title: 关于跨域问题
date: 2019-05-22 17:28:51
tags: 跨域
categories: 技术
---

## 什么是跨越
跨域，指的是浏览器不能执行其他网站的脚本。它是由 `浏览器的同源策略` 造成的，是 `浏览器` 施加的安全限制。
直接在终端 CURL 或使用 http 请求工具(postman)访问接口是不存在跨域的。
所谓同源是指，`域名` ，`协议` ，`端口` 均相同，不明白没关系，举个栗子：
```
http://www.dev.com/index.html 调用 http://www.dev.com/server.php （非跨域）

http://www.dev.com/index.html 调用 http://www.pro.com/server.php （主域名不同:dev/pro，跨域）

http://test.dev.com/index.html 调用 http://api.dev.com/server.php （子域名不同:test/api，跨域）

http://www.dev.com:8080/index.html 调用 http://www.dev.com:8081/server.php （端口不同:8080/8081，跨域）

http://www.dev.com/index.html 调用 https://www.dev.com/server.php （协议不同:http/https，跨域）
```
请注意：localhost和127.0.0.1虽然都指向本机，但也属于跨域。
## webpack 的 devServer 跨域原理
代码如下
```
devServer:{
  proxy: {
    '/api/*': {
      changeOrigin: true,
      target: 'https://testapi.qq.cn/',
      secure: false,
    }
  },
  disableHostCheck: true,
  inline: true,
  host: "0.0.0.0"
}
```
它的原理是，webpack 本身会创建一个 httpServer, 你浏览器所有访问 /api/ 的求情都会转发到 https://testapi.qq.cn/api，而不是直接通过浏览器向服务端发起请求的
## 关于配置
### Nginx
```
add_header Access-Control-Allow-Origin "*";
add_header Access-Control-Allow-Headers "Content-Type";
add_header Access-Control-Allow-Credentials true;
add_header Access-Control-Allow-Headers X-Requested-With;
add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS;
```
如果 reponse headers 出现`Access-Control-Allow-Origin "*, *"`, 必定是后端在业务代码中设置了header为*，请后端删除
### Javascript
```
// 是否允许跨域请求
// no-cors, cors, same-origin
// 默认值：same-origin. 同域
mode: 'cors'

// 是否携带 Cookie
// include, same-origin, omit
// 默认值: omit. 不携带
credentials: 'include'
```
如果想要跨域，请不要设置 cookie 的携带，会报错：
在Access-Control-Allow-Credentials设置为true的时候，Access-Control-Allow-Origin不能被设置为通配符。
因为 cookie 是同源机制，不允许跨域。
不知道后端在业务代码中写 Access-Control-Allow-Credentials：true 会不会有效？
