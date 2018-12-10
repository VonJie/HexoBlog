---
layout: _posts
title: HTML meta 标签
date: 2016-05-26 14:26:47
tags: meta
categories: 技术
---

``` html
<meta charset='utf-8'>
<!-- 声明文档使用的字符编码 -->

<html lang="zh-cmn-Hans">
<!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->

<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<!-- 优先使用 IE 最新版本和 Chrome -->

<meta name="renderer" content="webkit">
<!-- 360使用Google Chrome Frame，360 浏览器就会在读取到这个标签后，立即切换对应的极速核 -->

<meta http-equiv="Cache-Control" content="no-siteapp" />
<!-- 百度禁止转码 -->

<meta name="keywords" content="your keywords">
<!-- 页面关键词 keywords -->

<meta name="description" content="your description">
<!-- 页面描述内容 description -->

<meta name="author" content="author,email address">
<!-- 定义网页作者 author -->

<meta name="robots" content="index,follow" />
<!-- 搜索引擎抓取 -->


<!-- 移动端 -->
<meta name ="viewport" content ="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no">
<!-- 为移动设备添加 viewport -->

<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- 是否启用 WebApp 全屏模式 -->

<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
<!-- 设置状态栏的背景颜色，只有在 `"apple-mobile-web-app-capable" content="yes"` 时生效 -->

<meta name="format-detection" content="telephone=no" />
<!-- 禁止数字识自动别为电话号码 -->

<link rel="shortcut icon" type="image/ico" href="/favicon.ico" />
<!-- 添加 favicon icon -->

<meta name="google" value="notranslate" />
<!-- 禁止谷歌翻译 -->
```