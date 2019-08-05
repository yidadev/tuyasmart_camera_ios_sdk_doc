# 二维码配网

首先确定您已经接入了 TuyaSmartHomeKit（点击[这里](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/)查看接入文档）。

### 获取当前 WiFi 的SSID

```objective-c
self.ssid = [TuyaSmartActivator currentWifiSSID];
```

### 获取配网Token

```objective-c
long long homeId = '使用当前家庭的ID';
[[TuyaSmartActivator sharedInstance] getTokenWithHomeId:homeId success:^(NSString *result) {
self.token = result;
} failure:^(NSError *error) {
}];
```

### 组建二维码信息

```objective-c
// self.ssid
// self.token
// self.pwd        // 当前的WiFi密码，由用户输入
NSDictionary *dictionary = @{
@"s": self.ssid,
@"p": self.pwd,
@"t": self.token
};
NSData *jsonData = [NSJSONSerialization dataWithJSONObject:dictionary options:0 error:nil];
self.wifiJsonStr = [[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding];
```

### 开始配网

使用上面生成的 wifiJsonStr 生成二维码，确定设备处于配网状态，将二维码对准摄像头，设备捕捉到二维码信息后会发出提示音。此时通过以下接口开始监听配网结果。

```objective-c
[[TuyaSmartActivator sharedInstance] startConfigWiFi:TYActivatorModeQRCode ssid:self.ssid password:self.pwd token:self.token timeout:100];
```

### 停止配网

```objective-c
[[TuyaSmartActivator sharedInstance] stopConfigWiFi];
```

### 配网结果回调

配网结果通过代理 ```TuyaSmartActivatorDelegate```回调，代理方法如下：

```
// deviceModel 则为配网成功的设备的信息
- (void)activator:(TuyaSmartActivator *)activator didReceiveDevice:(TuyaSmartDeviceModel *)deviceModel error:(NSError *)error;
```

