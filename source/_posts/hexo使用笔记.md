---
title: Hexo 使用笔记
date: 2016-08-08 10:57:45
category: 
- 杂项
- 博客
tags:
- hexo
---

hexo 使用过程中的一些笔记，使用小技巧等等
<!--more-->

参考： [官方文档](https://hexo.io/zh-cn/docs/commands.html)

## 安装    
1. 安装[NodeJs](https://nodejs.org/zh-cn/)    
2. 运行    `npm install -g hexo-cli` 安装    

## [基本命令-点击跳转到官方中文说明](https://hexo.io/zh-cn/docs/commands.html)    
* 开启服务： 
```
hexo server [-i 192.168.24.124] [-l] [-o] [-p 4003] [-s]
hexo server -p 4003
```

* 部署到服务器    
```
hexo deploy
```

## 主题    
官方页面: [hexo 主题](https://hexo.io/themes/)


## 问题    
* 博客迁徙    
    在新的PC环境下clone下github上的项目时，运行`hexo` 命令时提示如下错误： 
```shell
ERROR Local hexo not found in E:\blog
ERROR Try running: 'npm install hexo --save'
```

此时在该目录下执行'npm install 即可'，原因是：.gitignore文件里面忽略了node_modules文件夹，所以这个文件夹没有更新上去。所以用npm重新安装即可。    

## 其他    

1. 使用 ``` <!--more-->``` 可以显示摘要,``` <!--more-->```以上部分会显示为摘要

2. 在hexo 中使用本地图片: [在 hexo 中无痛使用本地图片](http://www.tuicool.com/articles/umEBVfI)

3. [Markdown 语法中文版](http://www.appinn.com/markdown/)

4. [hexo 中文文档](https://hexo.io/zh-cn/docs/)


### 使用技巧
#### 插入图片(2018.04.12)    
1. `config.yml` 中开启 `post_asset_folder: true`
2. 在md文件同级目录下建立一个同名目录，然后放入图片，引用时按如下方式：    
  
```markdown    
    ![图片说明](./test.png)    
```    