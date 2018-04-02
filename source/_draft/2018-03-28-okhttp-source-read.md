---
layout: post
title: "OKHttp 源码阅读"
category :
- Android
tagline: "工作"
tags : [Android,OKHTTP]
---

## 下载源码

## 配置构建环境
### 1. 安装Maven :
```shell
cd ~/Applications
wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.tar.gz
tar -xvf apache-maven-3.5.3-bin.tar.gz
ln  -s ./apache-maven-3.5.3 maven

cp ~/.bash_profile ~/.bash_profile.bak
echo "export MAVEN_HOME=/Users/hanlyjiang/Applications/maven" >> ~/.bash_profile
echo "export PATH=$MAVEN_HOME/bin:$PATH" >> ~/.bash_profile
source ~/.bash_profile

mvn -v
```

