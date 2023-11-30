### WechatOpenSDK

[微信官方文档](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Access_Guide/iOS.html)

工程地址：https://github.com/xiaooojun/WechatOpenSDK.git

#### 主工程系统库添加

``` bash
Security.framework,
CoreGraphics.framework,
WebKit.framework
libz.tbd
libc++.tbd
libsqlite3.0.tbd
```

#### associated Domains 
添加link: applinks:域名

#### buildSetting 
Other Linker Flags 中加入 -ObjC、-all_load


#### URL Type配置 
info 底下 URL Type 添加 weixin wxxxxxkey

### info.plist 
添加 LSApplicationQueriesSchemes 配置weixin、weixinULAPI、weixinURLParamsAPI

#### 创建桥接文件

```swift
#import "WXApi.h"
#import "WXApiObject.h"
```
#### Code

```swift
    // AppDelegate <WXApiDelegate>
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        let appKey = "xxx"
        let linkUrl = ""
        WXApi.registerApp(appKey, universalLink: linkUrl)
        return true
    }
    
    // MARK: 微信调整相关方法
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        return WXApi.handleOpen(url, delegate: self)
    }
    
    func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {
        return WXApi.handleOpenUniversalLink(userActivity, delegate: self)
    }
    
    // MARK: WXApiDelegate
    func onReq(_ req: BaseReq) {
         print("请求：\(req)")
    }
    
    func onResp(_ resp: BaseResp) {
         print("响应: \(resp)")
    }
    
    // SceneDelegate <WXApiDelegate>
    func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
        WXApi.handleOpenUniversalLink(userActivity, delegate: self)
    }
    
    // 调用代码
    func shareGif() {
        let gifPath = Bundle.main.path(forResource: "spinner", ofType: ".gif")
        let gifUrl = URL(fileURLWithPath: gifPath ?? "")
        let gifData = try! Data(contentsOf: gifUrl)
        let emotion = WXEmoticonObject()
        emotion.emoticonData = gifData;
        
        let message = WXMediaMessage()
        message.mediaObject = emotion
        message.thumbData = gifData;
        
        let req = SendMessageToWXReq()
        req.bText = false
        req.message = message
        req.scene = 0;
        WXApi.send(req) { result in
            print(result)
        }
    }

```
