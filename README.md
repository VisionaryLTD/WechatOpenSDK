### WechatOpenSDK

[微信官方文档](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Access_Guide/iOS.html)

#### 主工程系统库添加

``` bash
Security.framework,
CoreGraphics.framework,
WebKit.framework
libz.tbd
libc++.tbd
libsqlite3.0.tbd
```

#### 链接参数配置
在"Other Linker Flags"中加入"-ObjC -all_load"

#### URL Type配置
在 Xcode 中，选择你的工程设置项，选中“TARGETS”一栏，在“info”标签栏的“URL type“添加“URL scheme”为你所注册的应用程序 id

### LSApplicationQueriesSchemes 配置
在Xcode中，选择你的工程设置项，选中“TARGETS”一栏，在“info”标签栏的“LSApplicationQueriesSchemes”添加weixin、weixinULAPI、weixinURLParamsAPI
