---
layout: post
title: "使用Github Pages 写博客"
category : 
- 杂项
- 博客
tagline: "记录在window上操作的过程"
tags : [Github,博客搭建]
---



按例在搭建完成后应该来一篇，不过为了避免重复造轮子，只写一些我操作过程中遇到的问题，并简单记录一下操作过程。其实之前搞过一次，使用的是octpress，这次用的是bootstrap。之前的博客地址：[Blog on the way]("https://libelosophy.github.com").

参考的一些文章：    
1. [Jekyll QuickStart-Host on GitHub in 3 Minutes](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)    
2. [GitHub Pages 搭建流程-基于jekyll-bootstrap](http://www.cnblogs.com/lslvxy/p/3402182.html) 在window上操作，现在正在windows操作。    
差不多就这了，之前在ubuntu上操作时遇到几个问题，都记录在ubuntu上。    
操作过程简单的写一下，主要写问题。

* ssh-keygen -t rsa -C "username@email.com" 

	生成公钥的时候，遇到了问题，如果使用cmd命令行环境，会找不到路径，在使用devkit中的msys作为命令行环境之后可以解决。
    
* gem install jekyll 出错 会提示某gz文件无法下载

    解决是看到了所指示[gem页面]("http://rubygems.org/pages/download")的一条命令：gem update --system ,之后再安装jekyll没有问题。
    

差不多了，主要就这两个问题。

最后说一个想法，我比较喜欢用手机随便记录点什么，也许可以利用git 的api 来写个手机应用。


更新一下提交到github后遇到的问题和一些记录：
    
1. 可以在本地使用jekyll 来build
2. 有时会出现上传后没有更新的情况，注意下邮箱中的通知，如：
The page build failed with the following error:
The file `_posts/2014-06-29-build-blog-with-github-pages.md` contains markdown errors. For more information, see https://help.github.com/articles/page-build-failed-markdown-errors.
If you have any questions please contact us at https://github.com/contact.
3. 一直出上面的错误，依照[github的help]("https://help.github.com/articles/migrating-your-pages-site-from-maruku")修改_config.yml再push就没有错了，整了半天。
添加一行：		
		markdown: kramdown

		
build blog with github pages using bootstrap