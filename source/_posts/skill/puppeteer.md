---
layout: _post
title: puppeteer
date: 2017-08-31 11:38:37
tags: puppeteer
categories: 技术
---

## 关于 `Puppeteer`
一个Node库，它提供了一组高级API，通过DevTools协议控制无界面Chrome; 默认headless也就是无UI的chrome，也可以配置为有UI; 有点类似于PhantomJS，但Puppeteer是Chrome官方团队进行维护的，前景更好。
[官网](https://pptr.dev/)
[GitHub](https://github.com/GoogleChrome/puppeteer)
[Chrome DevTools协议](https://chromedevtools.github.io/devtools-protocol/)
<!-- more -->
## `Chrome Headless`
```
// 设置别名
alias chrome="/Applications/Chrome.app/Contents/MacOS/Google\ Chrome"
// 在 linux 中，alias 命令（注意全为小写）的功能是设置命令的别名，以简写命令，提高操作效率

//打印DOM
chrome --headless --disable-gpu --dump-dom https://www.baidu.com

// 创建PDF
chrome --headless --disable-gpu --print-to-pdf https://www.baidu.com
// output.pdf

//截图（只能截取一屏，长页面无法截取）
chrome --headless --disable-gpu --screenshot --remote-debugging-port=9222 http://www.baidu.com
```

## 安装
注意 Node 版本
```
$ npm i puppeteer
```
## 使用
### 截图 / `PDF`
#### 代码
```
// index.js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
```
#### 运行
```
$ node index.js
```

### `Headless mode`
```
// 默认弹出浏览器
const browser = await puppeteer.launch({ ignoreHTTPSErrors: true, headless: false }); // default is true
```
### `Proxy`
```
let args = ['--no-sandbox', '--disable-setuid-sandbox', '--proxy-server=10.28.9.188:11421'];
const browser = await puppeteer.launch({ args: args });
```

### `设备型号`
[参考链接](https://github.com/GoogleChrome/puppeteer/blob/master/DeviceDescriptors.js)
```
await page.emulate(
  {
    'name': 'iPhone 6 Plus',
    'userAgent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 11_2_1 like Mac OS X) AppleWebKit/604.4.7 (KHTML, like Gecko) Mobile/15C153 MicroMessenger/6.6.1 NetType/WIFI Language/zh_CN',
    'viewport': {
      'width': 414,
      'height': 736,
      'deviceScaleFactor': 1,
      'isMobile': true,
      'hasTouch': true,
      'isLandscape': false
    }
  }
);
```

### `页面操作`
```
// 传参
let param = {};
await page.evaluate((param) => {
  // ...
  // 各种 js 操作
}, param)
```

## 服务端 `Lunix`
### 安装环境依赖
[解决方案](https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md)
#### CentOS
```
$ yum install pango.x86_64 libXcomposite.x86_64 libXcursor.x86_64 libXdamage.x86_64 libXext.x86_64 libXi.x86_64 libXtst.x86_64 cups-libs.x86_64 libXScrnSaver.x86_64 libXrandr.x86_64 GConf2.x86_64 alsa-lib.x86_64 atk.x86_64 gtk3.x86_64 ipa-gothic-fonts xorg-x11-fonts-100dpi xorg-x11-fonts-75dpi xorg-x11-utils xorg-x11-fonts-cyrillic xorg-x11-fonts-Type1 xorg-x11-fonts-misc
```
#### 测试运行
```
$ cd /node_modules/puppeteer/.local-chromium/linux-497674/chrome-linux
$ ./chrome --headless --disable-gpu --print-to-pdf https://www.baidu.com --no-sandbox
```
#### `NSS_VersionCheck("3.26") failed. NSS >= 3.26 is required.`
```
$ yum update nss
```
#### 中文乱码
```
yum install wqy-unibit-fonts.noarch wqy-zenhei-fonts.noarch -y
```