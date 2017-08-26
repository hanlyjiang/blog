---
layout: post
title: "使用SublimeText 编辑Markdown 文件并预览"
description: "使用SublimeText 编辑Markdown 文件并预览"
category: 
- 杂项
- Markdown
tags: 
- Markdown
---


<!-- toc -->

## SublimeText       
下载链接：    

SublimeText2:  http://www.sublimetext.com/2   
SublimeText3:  http://www.sublimetext.com/3   

选择对应系统的安装包下载就可以了，下载安装后在文件上右键就可以选择用SublimeText 编辑了。    

## 插件安装：
### 1. Package Control    
网址： https://packagecontrol.io/installation        

- 安装方式：        
    1 . 复制网页左边的一段python代码（根据SublimeText版本选择），在SublimeText中按_Ctrl+`_ ，然后Enter键回车。   
    2 . 退出重新打开。   
    3 . 此时就可以通过菜单中的 Preferences --> Package Control 来使用它了。  

### 2. Markdown Editing     
    通过 Preferences --> Package Control -->  Install Packages ,搜索安装    


### 3. Markdown Preview    
1. 安装：通过 Preferences --> Package Control -->  Install Packages ,搜索安装    
2. 使用：编辑Markdown文件时按 _Ctrl_+_B_  就可以将Markdown文件转换为Html文件        
3. 修改配置：         
    - 菜单 --> Preferences --> Package Settings -->  Markdown preview --> Settings - Default , _Ctrl_+_A_ 复制全部内容    
    - 菜单 --> Preferences --> Package Settings -->  Markdown preview --> Settings - Default , _Ctrl_+_V_ 粘贴        
    - 修改以下三项       
           
            "build_action": "browser",   
            "file_path_conversions": "relative",   
            "image_path_conversion": "relative",   

* 另外有个 Markdown Building 插件也是做相同的事情       


OK,搞定，平常做笔记，写文章什么的就很方便了。   



## 其他链接：    
- Markdown 语法说明： <http://www.markdown.cn/>
- [20 个强大的 Sublime Text 插件][link2]    
- [Sublime Text 3 支持的热门插件推荐][link3]

[link2]: http://www.oschina.net/translate/20-powerful-sublimetext-plugins
[link3]: http://www.imjeff.cn/blog/146/

