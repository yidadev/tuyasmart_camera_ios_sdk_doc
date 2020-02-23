## 快速集成

### 对接方式

在接入涂鸦 IPC SDK 之前，您可以先了解一下[涂鸦全屋智能 APP SDK](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/)，涂鸦 IPC SDK 需要依赖涂鸦全屋智能 APP SDK 其中的一部分，下面的文档也会介绍到依赖的这一部分。

涂鸦平台的对接方式分云云对接和全量涂鸦智能 SDK 对接两种，由于目前涂鸦 IPC 不支持完全的云云对接，在使用云云对接方案时，也需要接入涂鸦 IPC SDK，但基于这两种对接方式，接入涂鸦 IPC 时的 SDK 也略有不同。

涂鸦 IPC SDK 使用 CocoaPods 集成。

#### 云云对接

在 ```Podfile```文件中添加以下内容：

```ruby
platform :ios, '9.0'

target 'your_target_name' do

  pod "TuyaSmartActivator"
	pod "TuyaSmartCameraKit"
	# pod "TuyaSmartCameraT"

end
```

#### SDK 对接

在```Podfile```文件中添加以下内容：

```ruby
platform :ios, '9.0'

target 'your_target_name' do

  pod "TuyaSmartActivatorKit"
	pod "TuyaSmartCameraKit"
	# pod "TuyaSmartCameraT"

end
```

如果您需要支持 p2p 1.0 的设备，需要增加 ```pod "TuyaSmartCameraT"```, 然后在项目根目录下执行```pod update```命令。

CocoaPods的使用请参考：[CocoaPods Guides](https://guides.cocoapods.org/)

### 准备工作

在开始使用 SDK 开发前，需要在涂鸦智能开发平台上注册开发者账号、创建产品等，并获取到激活 SDK 的密钥，具体的操作流程请参考 [准备工作](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Preparation.html)。

### 初始化 SDK

1. 打开项目设置，Target => General，修改```Bundle Identifier```为涂鸦开发者平台上注册的 APP 对应的 iOS 包名。

2. 将上面[准备工作](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Preparation.html)中下载的安全图片导入到工程根目录，重命名为```t_s.bmp```，并加入「项目设置 => Target => Build Phases => Copy Bundle Resources」中。

3. 在项目的```PrefixHeader.pch```文件添加以下内容（Swift 项目可以添加在```xxx_Bridging-Header.h```桥接文件中）：

   ```objc
   #import <TuyaSmartBaseKit/TuyaSmartBaseKit.h>
   #import <TuyaSmartActivatorKit/TuyaSmartActivatorKit.h>
   #import <TuyaSmartCameraKit/TuyaSmartCameraKit.h>
   ```

4. 打开`AppDelegate.m`文件，在`[AppDelegate application:didFinishLaunchingWithOptions:]`方法中，使用在涂鸦开发者平台上，APP 对应的 `App Key`，`App Secret` 初始化SDK：

   __Objective-C__

   ```objc
   [[TuyaSmartSDK sharedInstance] startWithAppKey:<#your_app_key#> secretKey:<#your_secret_key#>];
   ```

   __Swift__

   ```swift
   TuyaSmartSDK.sharedInstance()?.start(withAppKey: <#your_app_key#>, secretKey: <#your_secret_key#>)
   ```

至此，涂鸦智能 App SDK 已经成功激活，可以开始 App 开发了。

### Debug 模式

在开发的过程中可以开启 Debug 模式，打印一些日志用于分析问题。

__Objective-C__

```objc
#ifdef DEBUG
    [[TuyaSmartSDK sharedInstance] setDebugMode:YES];
#else
#endif
```

__Swift__

```swift
#if DEBUG
   TuyaSmartSDK.sharedInstance()?.debugMode = true
#else
#endif

```

