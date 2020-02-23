# 涂鸦智能摄像头 iOS SDK

---

## 功能概述

涂鸦智能摄像头 SDK 提供了与远端摄像头设备通讯的接口封装，加速应用开发过程，主要包括了以下功能：

- 预览摄像头实时采集的图像。
- 播放摄像头SD卡中录制的视频。
- 手机端录制摄像头采集的图像。
- 与摄像头设备通话。

## SDK 简介

TuyaSmartCamera 在历史迭代版本中，有过多个 p2p 版本，从 p2p 1.0，p2p 2.0 到 p2p 3.0。 目前已经不再维护 p2p 1.0 ，```TuyaSmartCameraKit``` 中默认不再包含 p2p 1.0 的支持。如果您的 APP 需要支持 p2p 1.0 的设备，需要增加 ```TuyaSmartCameraT``` 的引用。设备的 p2p 类型可以通过 ```TuyaSmartDeviceModel``` 类中的 ```skills```属性中的值来判断。代码示例如下：

__Objective-C__

```objective-c
TuyaSmartDeviceModel *deviceModel;
id p2pType = [deviceModel.skills objectForKey:@"p2pType"];
if ([p2pType integerValue] == 1) {
    // p2p 1.0
}else {
    // p2p 2.0 或 p2p 3.0
}
```

__Swift__

```swift
let deviceModel:TuyaSmartDeviceModel!
let p2pType = self.device.deviceModel.skills["p2pType"]
if p2pType == 1 {
    // p2p 1.0
}else {
    // p2p 2.0 或 p2p 3.0
}
```

### 涂鸦 IPC 能力支持

* 设备配网
  * 快连模式（Smart Config）
  * 热点模式（AP 模式）
  * 二维码配网
* P2P 视频能力
  * 实时视频播放
  * SD 卡视频回放
  * 视频裸流数据
* 低功耗门铃
  * 电池管理
  * 休眠唤醒
  * 门铃呼叫
* 云存储
  * 云存储服务购买
  * 事件云存储
  * 云视频播放
* 报警消息
  * 消息列表
  * 音视频消息播放
* 扩展能力点
  * 云台控制
  * SD 卡管理
  * 侦测报警
  * 其他

以上能力视设备厂商不同有部分差异。

