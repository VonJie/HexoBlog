---
layout: _posts
title: css摘抄笔记
date: 2016-05-26 15:00:38
tags: csdn
categories: code
description: css摘抄笔记
---

```
/*清楚浮动*/
.clearfix{
    *zoom:1;
    _height:1px;
}
.clearfix:before, 
.clearfix:after { 
  display: table; 
  line-height: 0; 
  content: ""; 
} 

/*文字选中不高亮*/
.noselect{
    -webkit-touch-callout:none;
    -webkit-user-select:none;
    -khtml-user-select:none;
    -moz-user-select:none;
    -ms-user-select:none;
    user-select:none;
}

/*改变input[type="text"]的placeholder颜色*/
::-webkit-input-placeholder{
    color:#999;
}
:-moz-placeholder{
    color:#999;
    opacity:1;
}
::-moz-placeholder{
    color:#999;
    opacity:1;
}
:-ms-input-placeholder{
    color:#999;
}

/*水平垂直居中*/
div{
    display:table;
    width:250px;
    height:100px;
    text-align:center;
}
div span{
    display:table-cell;
    vertical-align:middle;
}
```