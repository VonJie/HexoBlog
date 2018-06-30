---
layout: _posts
title: emmet
date: 2018-06-28 11:05:47
tags: emmet
categories: web
description: emmet 相关用法
---

### 介绍
Emmet (前身为 Zen Coding) 是一个能大幅度提高前端开发效率的一个工具. 在前端开发的过程中，一大部分的工作是写 HTML、CSS 代码。特别是手动编写 HTML 代码的时候，效率会特别低下，因为需要敲打很多尖括号，而且很多标签都需要闭合标签等。于是，就有了 Emmet，它可以极大的提高代码编写的效率，它提供了一种非常简练的语法规则，然后立刻生成对应的 HTML 结构或者 CSS 代码，同时还有多种实用的功能帮助进行前端开发。
VsCode内置了Emmet语法,在后缀为.html/.css中输入缩写后按Tab键即会自动生成相应代码
> 请注意在VsCode新版本中按Tab不再默认启用Emmet展开缩写!需要在首选项配置中将emmet.triggerExpansionOnTab设置为true值!

### 语法
#### 快速新建 Html5 文档
```
// 将生成html5标准的包含body为空基本dom
!
html:5

// 生成XHTML过渡文档类型,DOCTYPE为XHTML
html:xt

// 生成HTML4严格文档类型,DOCTYPE为HTML 4.01
html:4s
```

#### Html 标签对
```
Element
```

#### id 属性 `Element#id`
```
section#app
=>
<section id="app"></section>
```

#### class 属性 `Element.class`
```
div.wrapper
=>
<div class="wrapper"></div>

div.test1.test2.test3
=>
<div class="test1 test2 test3"></div>
```

#### 特定属性 `Element[attr=foo]`
```
input[placeholder="please input your name"]
=>
<input type="text" placeholder="please input your name">

a[href='#' data-title='customer' target='_blank']
=>
<a href="#" data-title="customer" target="_blank"></a>
```

#### 标签包含内容 `Element{innerText}`
```
p{This is a paragraph}
=>
<p>This is a paragraph</p>
```

#### 嵌套操作符

##### 子元素 `Element>Element`
```
ul>li
=>
<ul><li></li></ul>
```

##### 同级元素 `Element+Element`
```
h1+p
=>
<h1></h1><p></p>
```
##### 上级元素 `Element^Element`
```
div>p.parent>span.child^ul.brother>li
=>
<div>
  <p class="parent"><span class="child"></span></p>
  <ul class="brother">
  <li></li>
  </ul>
</div>
```

#### 分组操作符 `()`
```
div>(ul>li+span)>a
=>
<div>
  <ul>
    <li></li>
    <span></span>
  </ul>
  <a href=""></a>
</div>
```

#### 乘法 `*`
```
ul>li*3
=>
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

#### 自动计数 `$`
```
ul>li.item${item number:$}*3
=>
<ul>
  <li class="item1">item number:1</li>
  <li class="item2">item number:2</li>
  <li class="item3">item number:3</li>
</ul>

ul>li.item$$*3{text :$$}
=>
<ul>
  <li class="item01">text :01</li>
  <li class="item02">text :02</li>
  <li class="item03">text :03</li>
</ul>
```
##### 修饰符_降序 `@-`
```
ul>li.item$@-*3
=>
<ul>
  <li class="item3"></li>
  <li class="item2"></li>
  <li class="item1"></li>
</ul>
```
##### 修饰符 降序:`@-`
```
ul>li.item$@-*3
=>
<ul>
  <li class="item3"></li>
  <li class="item2"></li>
  <li class="item1"></li>
</ul>
```
##### 修饰符 升序:`@+` 默认值
```
ul>li.item$@+*3
=>
<ul>
  <li class="item1"></li>
  <li class="item2"></li>
  <li class="item3"></li>
</ul>
```

##### 修饰符 起始值:`@N`
```
ul>li.item$@-10*3
=>
<ul>
  <li class="item12"></li>
  <li class="item11"></li>
  <li class="item10"></li>
</ul>
```

### 进阶高级用法
#### 模拟文本/随机文本
在开发时经常要填充一些文本内容占位,Emmet内置了Lorem Ipsum功能来实现.loremN或者lipsumN,N表示生成的单词数,正整数.可以不填.
```
lorem
=> Lorem ipsum dolor sit amet, consectetur adipisicing elit. Suscipit quia commodi vero sint omnis fugiat excepturi reiciendis necessitatibus totam asperiores, delectus saepe nulla consequuntur nostrum! Saepe suscipit recusandae repellendus assumenda.

p>lorem4
=>
<p>Lorem ipsum dolor sit.</p>

(p>lorem4)*3
=>
<p>Lorem ipsum dolor sit.</p>
<p>Labore aperiam, consequuntur architecto.</p>
<p>Quidem nisi, cum odio!</p>
```