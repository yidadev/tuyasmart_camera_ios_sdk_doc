# QRCode Network Configuration

Make sure you has access to TuyaSmartHomeKit (Click [HERE](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/en/) to view the document).

## Get Current WiFi SSID

__Objective-C__

``` objective-c
self.ssid = [TuyaSmartActivator currentWifiSSID];
```

__Swift__

``` swift
self.ssid = TuyaSmartActivator.currentWifiSSID()
```

## Get Token

__Objective-C__

```objective-c
long long homeId = 'current home id';
[[TuyaSmartActivator sharedInstance] getTokenWithHomeId:homeId success:^(NSString *result) {
self.token = result;
} failure:^(NSError *error) {
}];
```

__Swift__

``` swift
let homeId: Int64 = "current home id"
TuyaSmartActivator.sharedInstance()?.getTokenWithHomeId(homeId, success: { (result) in
    self.token = result
}, failure: { (error) in
})
```

## Build QR Code Information

__Objective-C__

```objective-c
// self.ssid
// self.token
// self.pwd        // current wifi password
NSDictionary *dictionary = @{
@"s": self.ssid,
@"p": self.pwd,
@"t": self.token
};
NSData *jsonData = [NSJSONSerialization dataWithJSONObject:dictionary options:0 error:nil];
self.wifiJsonStr = [[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding];
```

__Swift__

``` swift
let dictionary = [
    "s": self.ssid,
    "p": self.pwd,
    "t": self.token
]
let jsonData = JSONSerialization.data(withJSONObject: dictionary, options: JSONSerialization.WritingOptions.Element)
self.wifiJsonStr = String(data: jsonData, encoding: String.Encoding.utf8)
```

## Begin Network Distribute

Use the wifijsonstr to get qr code, make sure that the device is at network distribute status, put the qr code in front of cameram, device will have the indicate voice once it get the qr code information. mornitor the network distribute result with below interface:

__Objective-C__

```objective-c
[[TuyaSmartActivator sharedInstance] startConfigWiFi:TYActivatorModeQRCode ssid:self.ssid password:self.pwd token:self.token timeout:100];
```

__Swift__

``` swift
TuyaSmartActivator.sharedInstance()?.startConfigWiFi(TYActivatorModeQRCode, ssid: self.ssid, password: self.pwd, token: self.token, timeout: 100)
```

## Stop Network Distribute

__Objective-C__

```objective-c
[[TuyaSmartActivator sharedInstance] stopConfigWiFi];
```

__Swift__

``` swift
TuyaSmartActivator.sharedInstance()?.stopConfigWiFi()
```

## Call Back The Result 

Call back the result with the delegate: `TuyaSmartActivatorDelegate`, the delegate method :

__Objective-C__

``` objectivec
- (void)activator:(TuyaSmartActivator *)activator didReceiveDevice:(TuyaSmartDeviceModel *)deviceModel error:(NSError *)error;
```

__Swift__

``` swift
 func activator(_ activator: TuyaSmartActivator!, didReceiveDevice deviceModel: TuyaSmartDeviceModel!, error: Error!)
```
