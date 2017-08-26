---
title: Android Testing Support Library (2)- 配置测试环境
date: 2016-08-08 10:01:59
category: 
- Android
- 测试
tags: 
- Android
- Android 测试
---

本文内容：在Gradle 构建系统中使用Android Testing Support Library 
<!--more-->


## 在Gradle 构建系统中使用Android Testing Support Library 

在Android SDK Manager 中选择 __ Android Support Repository __ 条目，并下载安装,在Android Studio 中位置如下如图：

![Android Studio中下载方式](android_support_library_download.png)


下载完成后文件位于：<sdk>/extras/android/m2repository 目录下，而Android 测试支持库位于包 android.support.test下。

在Gradle 构建系统中使用Android 测试支持库时，你需要在build.gradle 文件中添加以下内容:

```
	dependencies {
	  androidTestCompile 'com.android.support.test:runner:0.4'
	  // 使用 JUnit 4 rules 需要
	  androidTestCompile 'com.android.support.test:rules:0.4'
	  // 使用 Espresso tests 需要
	  androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1'
	  // 使用 UI Automator tests 需要
	  androidTestCompile 'com.android.support.test.uiautomator:uiautomator-v18:2.1.2'
	}

```

在Gradle 构建系统中指定[AndroidJUnitRunner](https://developer.android.com/reference/android/support/test/runner/AndroidJUnitRunner.html) 为默认的测试Runner，在build.gradle 中defaultConfig 配置段中添加以下内容:
```
android {
    defaultConfig {
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
}

```
