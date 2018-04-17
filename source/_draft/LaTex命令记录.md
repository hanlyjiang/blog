---
title: LaTex 基本使用
date: 2018-4-16 09:30:40
tags:
- LaTex
---

<!-- TOC -->

### 1. 配置    
#### 中文支持及中文字体设置 
```Tex
% documentclass 使用ctex的document类，如ctexrep，ctexbook，ctexart
\documentclass[12pt,UTF8]{ctexrep}

%引入xeCJK包以设置中文字体
\usepackage{xeCJK}

%设置中文字体族 
\setCJKmainfont{NotoSansCJKsc-Light}[
ItalicFont = NotoSerifCJKsc-Light,  % 斜体用思源宋体显示
BoldItalicFont  = NotoSerifCJKsc-Bold
]  
%设置中文无衬线字体	
\setCJKsansfont{NotoSansCJKsc-Light}
%设置中文等宽字体
\setCJKmonofont{NotoSansMonoCJKsc-Regular}   


% 设置西文字体
\setmainfont{Helvetica} 
\setsansfont{Helvetica}
\setmonofont{Courier}
```

> 说明：    
> CJK 是 [中日韩统一表意文字](https://zh.wikipedia.org/wiki/CJK%E5%AD%97%E4%BD%93%E5%88%97%E8%A1%A8) （C-中国；J-日本；K-韩国）

#### **注释**:
- 单行注释： `%`
    ```Tex
    `%注释内容`
    ```
- 多行注释：
    ```Tex
    % 引入注释包
    \usepackage{comment}
    \begin{comment}
    注释内容
    \end{comment}
    ``` 

#### 定义颜色 + 设置超链接颜色
```Tex
% 定义颜色
\usepackage{xcolor}
% 定义一个名为 mtBlue 的颜色，色值为 229A93
\definecolor{mtBlue}{HTML}{229A93}

% 设置超链接颜色
\usepackage{hyperref}
\hypersetup{colorlinks,
	urlcolor=mtBlue,
	linkcolor=mtBlue
	}
```
> 参考：
> * [wikibooks-Hyperlinks  设置超链接](https://en.wikibooks.org/wiki/LaTeX/Hyperlinks#\href)
> * [latex-hyperref-doc](http://mirrors.ustc.edu.cn/CTAN/macros/latex/contrib/hyperref/doc/manual.html)


#### 设置行间距段落间距    
```Tex
% 设置行间距为2倍行距
\usepackage{setspace}
\doublespacing
% 设置段落间距
\setlength{\parskip}{0.25\baselineskip}
```

---------
    
### 2. 基本语法
以下内容仅针对 documentclass 为 article/report/book 类型的文档    

#### 空格及换行：
```Tex
\ %空格 
\\ % 换行
```


### * 参考资料    