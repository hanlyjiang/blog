---
title: Android Gradle 常用配置记录
date: 2018-4-16 13:27:40
tags:
- Android
- Tools
---


####  Release 版本可调试   
```groovy
 buildTypes {
        release {
            minifyEnabled false
            debuggable true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.geostar
        }
    }
```
