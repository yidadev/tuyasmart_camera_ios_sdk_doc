# 设备功能点

设备功能点定义了当前设备的状态，称作数据点（DP点），详细的定义与介绍查看[这里](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/zh-hans/resource/Device.html#%E8%AE%BE%E5%A4%87%E5%8A%9F%E8%83%BD%E7%82%B9)。

在[涂鸦开发者平台](https://iot.tuya.com/login)查询产品功能点的定义，如假设开始云台控制的功能点 id 为 “119”，停止云台控制的功能点 id 为 “116”，设备端以 “0”、“4”、“6”、“2” 标记上下左右四个方向，那么云台控制的示例代码如下：

```objective-c
// self.device = [TuyaSmartDevice deviceWithDeviceId:@"your_device_id"];

// 是否支持云台控制
- (BOOL)isSupportPtz {
	if ([self.device.deviceModel.dps objectForKey:@"119"]) {
		return YES;
	} else {
		return NO;
	}
}

// 开始云台控制
- (void)startPtz:(UISwipeGestureRecognizerDirection)direction {
	NSString *d = @"0";
	if (direction == UISwipeGestureRecognizerDirectionRight) {
		d = @"2";
	} else if (direction == UISwipeGestureRecognizerDirectionLeft) {
		d = @"6";
	} else if (direction == UISwipeGestureRecognizerDirectionUp) {
		d = @"0";
	} else if (direction == UISwipeGestureRecognizerDirectionDown) {
		d = @"4";
	}
	NSDictionary *dps = @{@"119": d};
	// 下发功能点
	[self.device publishDps:dps success:^{
		NSLog(@"publishDps success");
	//下发成功，状态上报通过 deviceDpsUpdate 方法回调
	} failure:^(NSError *error) {
		NSLog(@"publishDps failure: %@", error);
	}];
}

// 停止云台控制
- (void)stopPtz {
	NSDictionary *dps = @{@"116": @(YES)};
	// 下发功能点
	[self.device publishDps:dps success:^{
        //下发成功，状态上报通过 deviceDpsUpdate方法 回调
		NSLog(@"publishDps success");
	} failure:^(NSError *error) {
		NSLog(@"publishDps failure: %@", error);
	}];
}
```
