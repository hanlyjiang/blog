---
title: iOS 企业应用内部发布
date: 2018-06-09 09:47:45
category :
- iOS
tagline: "iOS"
tags : [iOS]
---

> 文章基于[iOS 企业内部(In-House)应用分发-简书](https://www.jianshu.com/p/89d22b430330) 修改   

* __企业内部应用:__
企业内部应用，即只在企业部门和员工内部使用、不对外公开的应用。苹果提供了专门的In-House证书用来发布这种应用，可以分发给任意的手机，只要通过一个URL即可下载安装，不用上传到App Stroe审核。我把企业内部应用也叫做In-House应用。
<!-- more  -->
* **In-House应用**: 有时需要根据部门需求进行版本的快速迭代，因为不需要App Store审核，所以可以做到随时修改，随时发布，节省了大量的时间。In-House证书还可以用于应用的内测分发，现在大部分的内测分发平台如蒲公英，Fir.im等的公测功能就是使用In-House证书实现的。

* **必须具备的两个条件：**
企业开发者账号。99$的普通开发者账号不行，必须以企业的名义申请一个299$的企业开发者账号Apple Developer Enterprise Program
带SSL证书的域名。企业内部应用需要把ipa文件上传到服务器，然后通过一个链接来下载安装，而苹果很重视安全性，要求这个链接的域名必须具有SSL证书，支持 https ，否则无法安装。
SSL证书其实并不是必需的，可以使用一些知名的云存储服务，比如亚马逊的AWS，阿里云等，也可以使用github,gitee,gitlab等代码托管仓库，这些大公司的云存储都支持Https，但299$的企业开发者账号就避免不了。

### 操作环境
* Mac High Sierra(10.13.4)
* MacBook Pro (Retina, 15-inch, Mid 2015)
* XCode Version 9.2 (9C40b)

### 内部分发需要的文件
* ipa文件。
* plist文件。名称必须与ipa文件一致，用于配置bundle id、版本号、ipa文件的URL、应用图标等。
* @1x 和 @2x 的Icon。下载安装时显示应用图标。

### 打包及发布过程

* 创建发布证书(Production Certificates)，选择In-House类型的。
* 创建配置文件(Distribution Provisioning Profiles)     
* 在Xcode选择对应的Code Signing 和 Provisioning Profile
![配置](https://note.youdao.com/yws/public/resource/962d8d3e55f80202a91625cc1015620d/xmlnote/3D097706FF384ED0AAD35A04F596A987/1589)

* 菜单 - Product - Archive，
然后出现如下界面，点击`export`，选择相应设置导出，导出过程参考：[iOS 企业内部(In-House)应用分发](https://www.jianshu.com/p/89d22b430330)    
![Archive-Export](https://note.youdao.com/yws/public/resource/962d8d3e55f80202a91625cc1015620d/xmlnote/390A96175B9D41739AB91F3607D11D2B/1604)    
> **提示：**:操作过程中会提示输入 `ipa` 链接，`display-image` 链接，`full-size-image` 链接。   
> 可以预先在gitee上建立一个仓库，如名为`appdist`的仓库，然后可以输入类似如下链接（hanlyjiang/appdist为用户名和仓库名称，替换为相应的链接）：   
> * ipa: https://gitee.com/hanlyjiang/appdist/raw/master/app.ipa
> * display-image: https://gitee.com/hanlyjiang/dayada/raw/master/image.57x57.png
> * full-size-image: https://gitee.com/hanlyjiang/dayada/raw/master/image.512x512.png    
> 当然，也可以后期直接编辑 plist 文件 

* 导出完成后，会有ipa文件和 `mainfest.plist` 文件生成，加入对应的图标   

#### 发布
* 文件和图标一起上传AWS的S3云存储，或gitee，或github，或 gitlab 上然后获取可以下载的地址。
如 gitee 地址：
`https://gitee.com/hanlyjiang/iospub/raw/master/manifest.plist`
* 发布一个web页面，加入如下链接：
`itms-services://?action=download-manifest&url=https://gitee.com/hanlyjiang/dayada/raw/master/manifest.plist`，可以用`<a>`标签包裹，最好在点击做出响应提示用户正在进行什么操作，可以参考蒲公英       


#### 安装
iOS的企业内部应用是通过访问plist文件来安装的，因为plist文件中包含了对应的ipa文件和图标的URL，iPhone会根据URL自动下载并安装应用程序。

在iPhone的Safari浏览器中打开之前的web页面：

首先会询问是否打开要打开链接，点击“打开”，然后询问是否要安装App，点击“安装”


### 不信任的企业开发者
如果你的手机是 iOS 9 及以上系统，那么第一次打开企业级应用时，你并不能打开App，提示需要手动信任开发者证书，iOS 9 以后，安装的企业级应用在第一次打开之前必须要用户手动去信任这些App，参考 [Guidelines for installing custom enterprise apps on iOS](https://support.apple.com/en-us/HT204460)。具体步骤如下：    
> 打开 `设置` --> `通用` --> `描述文件与设备管理`
在 企业级应用 分组下，点击 信任 开发者的证书


### 附：
* 如何在Xcode中打开archive窗口：
![image](https://note.youdao.com/yws/public/resource/962d8d3e55f80202a91625cc1015620d/xmlnote/D9D22A96290D43A282D8E0091AC869EB/1537)

### 参考资料
* [以无线方式安装企业内部应用/help.apple.com ](https://help.apple.com/deployment/ios/?spm=a2c4e.11153940.blogcont5039.5.160d30c0tdaSdZ#/apda0e3426d7)
* [Trust manually installed certificate profiles in iOS/support.apple.com](https://support.apple.com/en-au/HT204477)
* [itms-services ad hoc deployment problem/discussions.apple.com](https://discussions.apple.com/thread/5128050)

