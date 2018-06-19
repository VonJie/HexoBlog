---
title: 使用Hexo搭建个人博客
tag: 
- hexo
categories: 
- 前端开发
description: 
- 本文用于记录使用Hexo搭建个人博客的流程.
---

## 一. [新建 github 仓库](https://github.com/Wenj2)

#### Repository name

```
$ username.github.io
```

#### Description

``` 
$ 使用hexo搭建的个人博客
```

##### Initialize this repository with a README

```
$ check
```

## 二. [Hexo](https://hexo.io/zh-cn/)

#### 安装
```
$ npm install hexo-cli -g

$ hexo -v
```

#### 初始话项目
```
$ hexo init myHexoBlog

$ cd myHexoBlog

$ git clone https://github.com/iissnan/hexo-theme-next.git themes/next // 下载 next 主题
```
More info: [github](https://github.com/hexojs/hexo)

#### 命令
```
$ hexo s/server // 启动服务

$ hexo g/generate  // 生成静态文件

$ hexo d/deploy  // 部署、上传至 git；不使用，存在bug

$ hexo clean // 清理缓存
```

#### 配置 github SSH
```
$ cd ~/.ssh

$ vi id_rsa.pub  // 复制公钥到 guthub SSH keys
```

#### 上传文件至 github
```
$ cd public  // 所有生成的静态文件

$ git remote add origin https://github.com/Wenj2/Wenj2.github.io.git  // 添加远程仓库地址

$ git pull origin master --allow-unrelated-histories  // 强制合并

$ git push origin master
```

## 三. [Hexo 相关配置](https://hexo.io/zh-cn/docs/)

##### 配置 根目录 _config.yml
```
$ theme: next  // 配置主题

$ language: zh-Hans  // 设置语言
```

## 四. [next 主题配置](http://theme-next.iissnan.com/getting-started.html)

#### 配置 themes/next/_.config.yml
```
$ footer: 
    copyright: ❤ Power by - Jesse  // 配置页面底部内容
  
$ menu  // 配置导航菜单

$ scheme: Pisces  // 选择 next 下的主题
```

#### 配置 themes/next/layout/_partails/footer.swig

#### 配置 themes/next/languages/zh-Hans.yml

## 五. [图片相关](https://github.com/VonJie/images.git)

#### 新建 images 仓库
- ##### 新建分类路径
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/hexo/WX20180608-112836.png)
- ##### 在文件后带 ' / ' ，会自动生成路径 
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/hexo/WX20180608-112928.png)
- ##### 上传文件
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/hexo/WX20180608-112952.png)
- ##### 选择文件
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/hexo/WX20180608-113019.png)
- ##### 提交修改
![image](https://raw.githubusercontent.com/VonJie/images/master/blog/hexo/WX20180608-113042.png)