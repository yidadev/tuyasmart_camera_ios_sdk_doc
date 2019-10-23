# TuyaSmartCamera iOS SDK

---

## Features Overview

Tuya Smart Camera SDK provides the interface package for the communication with remote camera device to accelerate the application development process, including the following features:

* Preview the picture taken by the camera
* Play back recorded video of the remote camera
* Record video
* Talk to the remote camera

## SDK Introduction

In the historical version of TuyaSmartCamera, there have been multiple p2p versions, from p2p 1.0, p2p 2.0 to p2p 3.0, and support for p2p 1.0 is no longer included in TuyaSmartCameraKit by default. If your app needs supports p2p 1.0, you need to add a reference to TuyaSmartCameraT. The p2p type of the device can be determined by the value in the skills attribute in the TuyaSmartDeviceModel class.

__Objective-C__

```Objective-C
TuyaSmartDeviceModel *deviceModel;
id p2pType = [deviceModel.skills objectForKey:@"p2pType"];
if ([p2pType integerValue] == 1) {
    // p2p 1.0
}else {
    // p2p 2.0 or p2p 3.0
}
```

__Swift__

```Swift
let deviceModel:TuyaSmartDeviceModel!
let p2pType = self.device.deviceModel.skills["p2pType"]
if p2pType == 1 {
    // p2p 1.0
}else {
    // p2p 2.0 or p2p 3.0
}
```