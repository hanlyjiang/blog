---
title: 使用 Android 命令提高效率
date: 2018-4-16 13:27:40
tags:
- Android
- Tools
---



####  Activity Manager (am)
假设有应用包名为`com.hanlyjiang.test`,可以启动的Activity为： `com.hanlyjiang.test/.MainActivity`    

1. 关闭应用后再次启动应用   
```bash
adb shell am start -S am start -S com.hanlyjiang.test/.MainActivity
```
2. 停止应用
```bash
adb shell am force-stop com.hanlyjiang.test
```


####  软件包管理器 (pm)
```bash
adb shell am start 
```

#### 输出给定 package 的 APK 的路径。
```
pm  path package	
```
#### 将软件包（通过 path 指定）安装到系统。
```
pm install [options] path	
```
    选项：

    -l：安装具有转发锁定功能的软件包。
    -r：重新安装现有应用，保留其数据。
    -t：允许安装测试 APK。
    -i installer_package_name：指定安装程序软件包名称。
    -s：在共享的大容量存储（如 sdcard）上安装软件包。
    -f：在内部系统内存上安装软件包。
    -d：允许版本代码降级。
    -g：授予应用清单中列出的所有权限。

#### 从系统中移除软件包
```
uninstall [options] package	
```

    选项：

    -k：移除软件包后保留数据和缓存目录。
    clear package	删除与软件包关联的所有数据。


### 参考:
* [Am命令用法-gityuan](http://gityuan.com/2016/02/27/am-command/)
* [Pm命令用法-gityuan](http://gityuan.com/2016/02/28/pm-command/)
* [Google官方文档-Android 调试桥](https://developer.android.google.cn/studio/command-line/adb.html)