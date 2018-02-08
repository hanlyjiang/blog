# Android 端Word PDF查看

## 腾讯浏览服务
https://x5.tencent.com/tbs/guide/demo.html#

```java
public static int openFileReader(Context context, String filePath, HashMap<String, String> extraParams,ValueCallback<String> callback)
```
### 【接口说明】
1. 此方法在 Qbsdk 类下
2. 调用之后，优先调起 QQ 浏览器打开文件。如果没有安装 QQ 浏览器，在 X5 内核下调起简版 QB 打开文 件。如果使用的系统内核，则调起文件阅读器弹框。
3. 此方法暂时只支持本地文件打开，在线文件后期完善   


### 【参数定义】


`context`: 调起 miniqb 的 Activity 的 context。此参数只能是 activity 类型的 context，不能设置为 Application的 context。  

`filePath`: 文件路径。格式为 android 本地存储路径格式，例如: `/sdcard/Download/xxx.doc`. 不支持 `file:///` 格式。暂不支持在线文件。

`extraParams`:miniqb 的扩展功能。为非必填项，可传入 null 使用默认设置。其格式是一个 key 对应一个 value。在文件查看器的产品形态中，当前支持 的 key 包括:
* `local`: “true”表示是进入文件查看器，如果不设置或设置为“false”，则进入 miniqb 浏览器模式。不是必须设置项。
* `style`: “0”表示文件查看器使用默认的 UI 样式。“1”表示文件查看器使用微信的 UI 样式。不设置此 key或设置错误值，则为默认 UI 样式。
* `topBarBgColor`: 定制文件查看器的顶部栏背景色。格式为“#xxxxxx”，例“#2CFC47”;不设置此 key 或设置错误值，则为默认 UI 样式。
* `menuData`: 该参数用来定制文件右上角弹出菜单，可传入菜单项的 icon 的文本，用户点击菜单项后，sdk会通过 startActivity+intent 的方式回调。menuData 是 jsonObject 类型，结构格式如下:
```java
public static final String jsondata =
    "{
    pkgName:\"com.example.thirdfile\", "
    + "className:\"com.example.thirdfile.IntentActivity\","
    + "thirdCtx: {pp:123},"
    + "menuItems:"
    + "["
    + "{id:0,iconResId:"+ R.drawable.ic_launcher +",text:\"menu0\"},
    {id:1,iconResId:" + R.drawable.bookmark_edit_icon + ",text:\"menu1\"}, {id:2,iconResId:"+ R.drawable.bookmark_folder_icon +",text:\"菜单2\"}" + "]"
    +"
    }";
```
* `pkgName` 和 className 是回调时的包名和类名。thirdCtx 是三方参数，需要是 jsonObject 类型，sdk 不会处理该参数，只是在菜单点击事件发生的时候原样 回传给调用方。menuItems 是 json 数组，表示菜单中的每一项。

* `ValueCallback`:提供 miniqb 打开/关闭时给调用方回调通知,以便应用层做相应处理。 在单独进程打开文件的场景中，回调参数出现如下字符时，表示可以关闭当前进程，避免内存占用。
 openFileReader open in QB
    filepath error
    TbsReaderDialogClosed
    default browser:
    filepath error
    fileReaderClosed

### 【返回说明】
1:用 QQ 浏览器打开    
2:用 MiniQB 打开  
3:调起阅读器弹框  
1:filePath 为空 打开失败

###【使用实例】
```java
    //params 为定制参数 非必须选项 可以传 null 为默认设置 public static final Stringjsondata = "{
    pkgName:\"com.example.thirdfile\", "
    + "className:\"com.example.thirdfile.IntentActivity\","
    + "thirdCtx: {pp:123},"
    + "menuItems:"
    + "["
    + "{id:0,iconResId:"+ R.drawable.ic_launcher +",text:\"menu0\"},
    {id:1,iconResId:" + R.drawable.bookmark_edit_icon + ",text:\"menu1\"}, {id:2,iconResId:"+ R.drawable.bookmark_folder_icon +",text:\"菜单 2\"}"
    + "]" + " }";
    HashMap<String, String> params = new HashMap<String, String>(); params.put("style", "1");
    params.put("local", "true");
    params.put("memuData", jsondata);
    QbSdk.openFileReader(ctx,”/sdcard/xxx.doc”, params,callback);
```



public static final String packageName = "\"com.geostar.tbsdemo\"";
public static final String activityName = "\"com.geostar.tbsdemo.FileActivity\"";
